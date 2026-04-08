# STM32 Black Pill (STM32F411CEU6) Learning Journey 

Welcome to my personal STM32 Black Pill repository! 

I created this space to document my journey of learning and experimenting with the STM32F411CEU6 microcontroller. I am a big believer in getting down to the barebones of how hardware works, so as I learn, I'll be logging all my code, notes, and guides right here. 

Whether you are a student, a fellow embedded enthusiast, or just my future self trying to remember how to configure a timer—this repo is for you. Feel free to use the code, try out the builds, and learn alongside me.

---

## 📖 The Origin Story: Down the Rabbit Hole

When I first got my hands on the Black Pill, I didn't entirely know what features it was packing under the hood. So, I did what any embedded developer does: I went straight to the STMicroelectronics website, searched for the chip, and downloaded the holy grail—the [STM32F411CEU6 Datasheet](https://www.st.com/en/microcontrollers-microprocessors/stm32f411ce.html).

While trying to understand the chip's architecture and pinouts using the datasheet, I had an "aha!" moment. I realized that the datasheet is just the tip of the iceberg. To truly master this silicon, I needed to explore the heavier documentation. 

I won't lie, reading through these next few sources drove me a little crazy at first. But making it through them genuinely made me a completely different person in the embedded world. If you want to understand the *why* behind the code, you need to read these:

* **[STM32F411 Reference Manual](https://www.st.com/resource/en/reference_manual/DM00119316-.pdf):** The absolute bible for understanding how the registers and peripherals actually operate.
* **[Cortex®-M4 Technical Reference Manual](https://www.st.com/content/ccc/resource/technical/document/programming_manual/6c/3a/cb/e7/e4/ea/44/9b/DM00046982.pdf/files/DM00046982.pdf/jcr:content/translations/en.DM00046982.pdf):** Essential for understanding the core processor, interrupts (NVIC), and memory model.

---

## 📚 Good Reads & Visuals

Sometimes you need more than just dry technical manuals. Here are some of the best reads and visual guides that made this journey much more wonderful and intuitive:

* **[GPIO Programming Guide](https://embetronicx.com/tutorials/tech_devices/understanding-the-microcontroller-gpio-gpio-working-explained/):** A fantastic resource that helped visualize and lock in my understanding of General Purpose I/O on the STM32.

* **[Guide: Connecting your debugger](https://stm32-base.org/guides/connecting-your-debugger.html):** Before you can flash code, you have to figure out how to actually talk to the board. This guide is a lifesaver—it breaks down the exact pinouts and physical connection details for pretty much every type of ST-Link clone and original debugger out there.

*(I'll be updating this list with more resources as I find them!)*

---

Adding more info as I explore more ...... 

stay Embedded till then 😎