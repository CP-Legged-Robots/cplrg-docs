Lithium-Ion (Li-ion) batteries are another common rechargeable battery chemistry 
used in robotics. Like [[LiPo Batteries]], they offer high energy density and low 
mass, making them well suited for mobile robotic systems. 

Li-ion batteries generally have higher energy density than LiPo batteries, 
meaning they can store more energy per unit mass. This makes them advantageous 
for applications where long runtime is prioritized over peak power output. 
However, Li-ion cells are typically more limited in discharge rate, meaning they 
cannot safely supply extremely high currents without voltage sag or overheating. 
To achieve higher current capability, multiple cells must be connected in 
parallel.

# Battery Management

Unlike most hobby LiPo packs, Li-ion battery packs almost always include a 
Battery Management System (BMS). The BMS:

* Prevents overcharge and over-discharge

* Limits current draw

* Balances cells in multi-cell configurations

* Protects against short circuits

This built-in protection makes Li-ion packs generally safer and more 
user-friendly for long-term deployment in robotic systems.

# Switch-2 Installation

The following forum shos the intended work required to fully adapt the Li-ions 
to Switch-2 for compatibility with the [[Power Distribition Board]]. 