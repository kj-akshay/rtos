Repository: 01_Blink_BareMetal (STM32F4) — copilot instructions

1) Build, test, and lint commands

- Build (local, GNU make + GNU Arm Embedded Toolchain):
  - From project root: `cd 01_Blink_BareMetal && make -f Debug\makefile`  
  - Or from Debug folder: `cd 01_Blink_BareMetal\Debug && make`  
  - Toolchain required: arm-none-eabi-gcc (GNU Arm Embedded Toolchain) and GNU make on PATH.

- Inspect artifacts:
  - `arm-none-eabi-size 01_Blink_BareMetal.elf`
  - `arm-none-eabi-objdump -h -S 01_Blink_BareMetal.elf > 01_Blink_BareMetal.list`

- Flashing / debugging:
  - No project-level flashing scripts found. Typical workflow: open the project in STM32CubeIDE (01_Blink_BareMetal.ioc/.project) and use the IDE's Run/Debug targets or use OpenOCD/other tooling externally.

- Tests & lint:
  - No automated test suite or linter configuration detected in the repository.
  - If adding tests, keep them in a dedicated tests/ folder and document how to run them here.

2) High-level architecture (big picture)

- Purpose: Simple bare-metal STM32F4 blink example scaffold (project name: 01_Blink_BareMetal).

- Top-level layout (important parts):
  - 01_Blink_BareMetal/  — CubeIDE project folder (sources & generated build files)
    - Core/Startup/startup_stm32f401retx.s  — vector table and reset handler
    - Core/Inc  — project public headers (main.h, stm32f4xx_hal_conf.h, irq headers)
    - Core/Src/main.c  — application entry / main loop
    - Drivers/STM32F4xx_HAL_Driver/  — HAL driver sources (vendor code)
    - Drivers/CMSIS/  — CMSIS headers and device headers
    - STM32F401RETX_FLASH.ld, STM32F401RETX_RAM.ld — linker scripts (flash & ram layouts)
    - 01_Blink_BareMetal.ioc  — CubeMX configuration used to generate HAL/init code
    - Debug/  — generated makefile, build outputs (.elf, .map, .list)

- Runtime flow (quick): reset -> startup assembly sets stack and vectors -> SystemInit/system_stm32f4xx.c runs -> HAL init -> main() performs MCU/peripheral setup and app loop.

3) Key conventions and repository-specific patterns

- CubeMX / CubeIDE generated layout: many build artifacts and makefiles are generated under Debug/. Do not commit IDE-specific ephemeral files (prefer keeping generated files out of PRs).

- Build artifact names: the produced ELF and related files are named `01_Blink_BareMetal.*` (see Debug/makefile).

- Linker scripts live at project root and are explicitly referenced by generated makefiles; changing target MCU or memory layout requires updating those .ld files and regen from CubeMX.

- Vendor code organization:
  - HAL driver sources under Drivers/STM32F4xx_HAL_Driver
  - CMSIS device headers under Drivers/CMSIS
  - Expect HAL subsystem usage patterns (HAL_Init, HAL_Delay, peripheral HAL_xxx functions) in application code.

4) Existing docs and assistant configs integrated

- No README.md, CONTRIBUTING.md, or other top-level developer docs detected.
- No AI assistant rules/agent files detected (CLAUDE.md, .cursorrules, AGENTS.md, .windsurfrules, CONVENTIONS.md, AIDER_CONVENTIONS.md, .clinerules).

5) Where to look next & common tasks for future Copilot sessions

- Editing behavior: open 01_Blink_BareMetal.ioc in STM32CubeIDE to regenerate configurations; major changes to MCU/peripheral config are expected to be driven through CubeMX.
- When adding CI or automation (recommended): provide a headless/build script that invokes `make -f Debug\makefile` from the project folder and ensures arm-none-eabi toolchain is available.

---

If this repository has other projects in the workspace, adjust paths above to target the specific project you want Copilot to operate on.
