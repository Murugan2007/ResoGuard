/* Murugan FOSSEE OSH 2026*/
/* Includes */
#include "main.h"
#include "arm_math.h"
#include "ssd1306.h"   // Standard OLED Library
#include "fonts.h"     // Font library for OLED
#include <stdio.h>
#include <string.h>

/* Constants & FFT Buffers */
#define SAMPLES 512
#define SAMPLING_FREQ 100
float32_t fft_input[SAMPLES * 2];
float32_t fft_output[SAMPLES];
float32_t magnitudes[SAMPLES / 2];

/* System Variables */
float32_t current_freq = 0.0f;
float32_t baseline_fn = 0.0f;
int16_t accel_data[3];
char display_buffer[32]; // Buffer for OLED text formatting

/* Function Prototypes */
void Run_FFT_Analysis(void);
void Update_OLED_Display(uint8_t alert_state);
void Trigger_Alarm(uint8_t state);

int main(void) {
  HAL_Init();
  SystemClock_Config();
  
  /* Peripherals Initialization */
  MX_GPIO_Init();
  MX_I2C1_Init(); // I2C for both Accelerometer AND OLED
  MX_TIM1_Init(); // Motor PWM (PA8)
  MX_TIM2_Init(); // Buzzer PWM
  MX_USART2_UART_Init(); // ESP32 Comms

  /* Start Subsystems */
  HAL_TIM_PWM_Start(&htim1, TIM_CHANNEL_1);
  ssd1306_Init(); // Initialize OLED

  // Startup Screen
  ssd1306_Fill(Black);
  ssd1306_SetCursor(10, 10);
  ssd1306_WriteString("ResoGuard", Font_11x18, White);
  ssd1306_SetCursor(10, 35);
  ssd1306_WriteString("Calibrating...", Font_7x10, White);
  ssd1306_UpdateScreen();
  
  // Give the bridge a "tap" to find the baseline natural frequency
  HAL_Delay(2000); 
  Run_FFT_Analysis();
  baseline_fn = current_freq; 

  while (1) {
    // 1. Analyze Vibrations
    Run_FFT_Analysis();

    // 2. Logic & Control
    uint8_t is_danger = 0;
    
    // If current frequency matches the bridge's natural resonance
    if (fabsf(current_freq - baseline_fn) < 0.5f && current_freq > 1.0f) {
        is_danger = 1;
        Trigger_Alarm(1); 
        // Send alert to ESP32 for Smart City Dashboard
        HAL_UART_Transmit(&huart2, (uint8_t*)"RESONANCE_DETECTED\n", 19, 100);
    } else {
        Trigger_Alarm(0);
    }

    // 3. Update Visuals
    Update_OLED_Display(is_danger);
    
    HAL_Delay(200); // UI Refresh Rate
  }
}

/* OLED UI Function */
void Update_OLED_Display(uint8_t alert_state) {
    ssd1306_Fill(Black); // Clear screen
    
    // Line 1: Show Baseline Resonance
    sprintf(display_buffer, "Base Fn: %.1f Hz", baseline_fn);
    ssd1306_SetCursor(5, 5);
    ssd1306_WriteString(display_buffer, Font_7x10, White);
    
    // Line 2: Show Current Frequency
    sprintf(display_buffer, "Live: %.1f Hz", current_freq);
    ssd1306_SetCursor(5, 20);
    ssd1306_WriteString(display_buffer, Font_7x10, White);

    // Line 3: Status Warning
    ssd1306_SetCursor(5, 40);
    if (alert_state) {
        ssd1306_WriteString("! DANGER !", Font_11x18, White); // Large Font
    } else {
        ssd1306_WriteString("Status: SAFE", Font_11x18, White);
    }
    
    ssd1306_UpdateScreen(); // Push to physical display
}

/* FFT Logic (Simplified) */
void Run_FFT_Analysis(void) {
    arm_rfft_fast_instance_f32 S;
    arm_rfft_fast_init_f32(&S, SAMPLES);

    for (int i = 0; i < SAMPLES; i++) {
        // Replace with your ST HAL Accelerometer read function
        fft_input[i*2] = (float32_t)accel_data[2]; // Z-Axis vibration
        fft_input[i*2 + 1] = 0;
        HAL_Delay(10); 
    }

    arm_rfft_fast_f32(&S, fft_input, fft_output, 0);
    arm_cmplx_mag_f32(fft_output, magnitudes, SAMPLES / 2);

    float32_t max_val;
    uint32_t max_index;
    arm_max_f32(magnitudes, SAMPLES / 2, &max_val, &max_index);
    
    current_freq = (float32_t)max_index * SAMPLING_FREQ / SAMPLES;
}

/* Buzzer Control */
void Trigger_Alarm(uint8_t state) {
    if(state) {
        __HAL_TIM_SET_COMPARE(&htim2, TIM_CHANNEL_1, 50); // Beep ON
        HAL_GPIO_WritePin(GPIOD, GPIO_PIN_13, GPIO_PIN_SET); 
    } else {
        __HAL_TIM_SET_COMPARE(&htim2, TIM_CHANNEL_1, 0);  // Beep OFF
        HAL_GPIO_WritePin(GPIOD, GPIO_PIN_13, GPIO_PIN_RESET);
    }
}
