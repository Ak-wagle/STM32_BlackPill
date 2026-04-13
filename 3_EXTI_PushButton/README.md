# 3 — EXTI Button Interrupt

> Ditch the `while(1)` polling loop. Configure an External Interrupt (EXTI) so the CPU only reacts to a button press instead of constantly checking it — freeing up the processor for real work.

---

## 🛠️ Hardware Required

| Component | Quantity |
| :--- | :--- |
| STM32F411CEU6 (Black Pill) | 1 |
| STLink V2 | 1 |

> Both the push button and the LED used here are **onboard** — no external components needed.

---

## 🔌 Circuit & Wiring

| Black Pill Pin | Connected To | Function |
| :--- | :--- | :--- |
| `PA0` | Onboard Push Button | GPIO EXTI Input |
| `PC13` | Onboard LED | GPIO Output |

---

## ⚙️ STM32CubeMX Configuration

**Pinout & Configuration:**
- Click `PA0` → Set as `GPIO_EXTI0`
- Click `PC13` → Set as `GPIO_Output`

**System Core → GPIO → PA0:**
- GPIO mode: `External Interrupt Mode with Falling edge trigger detection`
- GPIO Pull-up/Pull-down: `Pull-up`

**System Core → NVIC:**
- Enable `EXTI line0 interrupt` ✅ — don't skip this, the interrupt will never fire without it

**System Core → RCC / Clock Configuration:**
- Leave everything default — internal HSI at 16 MHz is sufficient for this project

---

## 🧠 Core Logic & HAL APIs

**Behavior:** Each button press toggles the LED — press once to turn ON, press again to turn OFF.

### How the interrupt flow works

```
Button pressed (PA0 falls LOW)
        │
        ▼
EXTI Line 0 fires
        │
        ▼
NVIC catches it, pauses while(1)
        │
        ▼
EXTI0_IRQHandler()        ← auto-generated in stm32f4xx_it.c
        │
        ▼
HAL clears the flag, calls...
        │
        ▼
HAL_GPIO_EXTI_Callback()  ← your code lives here
        │
        ▼
LED toggles → CPU returns to while(1)
```

### The ISR (auto-generated — do not modify)

```c
// stm32f4xx_it.c
void EXTI0_IRQHandler(void)
{
    HAL_GPIO_EXTI_IRQHandler(GPIO_PIN_0);
}
```

### Users callback in `main.c`

```c
/* USER CODE BEGIN 4 */
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
    if (GPIO_Pin == GPIO_PIN_0)
    {
        HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
    }
}
/* USER CODE END 4 */
```

### The `while(1)` loop — intentionally empty

```c
while (1)
{
    /* CPU is free — interrupt handles everything */
}
```

---

## 📝 Concepts & Gotchas

### ⚡ Polling vs. Interrupts — Why This Matters

| | Polling | EXTI Interrupt |
| :--- | :--- | :--- |
| **How it works** | CPU checks pin state every loop | CPU reacts only when edge occurs |
| **CPU usage** | Constantly busy checking | Free to idle or do other work |
| **Responsiveness** | Depends on loop speed | Immediate — hardware triggered |
| **Use case** | Simple, single-task sketches | Real-time, multi-task systems |

---

### 🔀 Why Toggle and Not Press=ON / Release=OFF?

EXTI fires on an **edge** — that single moment of transition — not on a sustained level. So the interrupt only tells you *something happened*, not *the button is currently held*. Toggle is the natural fit for a single-edge interrupt, So the toggle led exmple was taken here.

If you need press=ON / release=OFF behaviour, configure the pin for **Rising/Falling edge** detection and read the pin state inside the callback to determine which edge just fired.

---

### 🔘 Falling Edge — Why Not Rising?

PA0 is configured with a Pull-Up resistor, so it sits at **HIGH** at rest and goes **LOW** when the button is pressed. That HIGH→LOW transition is a **falling edge** — which is exactly what we want to trigger on. A rising edge would fire on button *release* instead.

---

### ⚠️ Button Bounce

Physical buttons don't produce a clean single edge — they bounce and can trigger the interrupt several times in one press.
Check about Debounce techniques.

---

## 📚 References

