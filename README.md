# Fly Swatter Counter Mod

Showcase and documentation. Not a 'how-to' or tutorial.

## Disclaimer
This project works with potentially dangerous electronics. All appropiate safety measures are not necessairly shown or explained. Do not try this at home.

No living beings were hurt in the making of this. I won't go into the ethics of fly swatters.

## Motivation
When asked what he wants for christmas, my father has had the same answer for years: "A fly swatter with a counter!". He himself started wondering about possible implementations and their problems, but has neither the means nor the knowledge to build such a thing.

This project begins 26 days before christmas, so this will be a one-off figure-it-out-as-you-go thing.

## The fly swatter
Used in this project is a fly swatter from 'Night Cat' sold on amazon [Night Cat Electric Mosquito Fly Swatter, Fly Catcher, Zapper, Insect Killer, USB Rechargeable, LED Lighting, Double-Layer Mesh Protection](https://amzn.eu/d/cAcfHg3). It has proven to be long lived and has an amount of oohmpf that makes me wonder how it is sold in the EU.

### Teardown
I was not able to find a teardown of this product on the internet, so I hope the follwing can pose helpful for other people modding or repairing their fly swatter.
![Full view of fly swatter](/readme_src/fly_swatter_full_view.png)

The swatter has three mesh layers. The outer ones are ground, the inner one is live.
As a safety measure, I made sure to discharge the swatter by connecting the different mesh layers with a screwdriver. Afterwards, I connected the middle layer to an outer one using crocodile clips.
![Layers of the swatter connected with a crocodile clip cable](/readme_src/connect_layers_with_crocodile_clips.jpg)

The back of the swatter has 8 equal phillips screws. Depending on what you want to do, you might not need to remove all of them.
![Back of the swatter with the screws removed](/readme_src/back_of_swatter_screws_removed.png)

Here is a closeup of the sticker.
![Closeup of sticker](/readme_src/sticker_closeup.png)

The bottom of the handle houses the battery and can be unscrewed.
![Battery handle unscrewed](/readme_src/battery_handle_unscrewed.png)

To my surprise, the battery case connects to the rest of the swatter through a barrel jack.
![Battery connector female](/readme_src/battery_connector_female.png)
![Battery connector male](/readme_src/battery_connector_male.png)

A single srew holds the battery cover. 10/10 for repairability, you can straight up replace this standard cell for an equal or even a larger one.
![Battery handle without cover](/readme_src/battery_handle_without_cover.png)

I measured the cell to be 4V (not fully charged).
![Battery voltage measurement](/readme_src/battery_voltage_measurement.png)
I am not familiar with battery cells, but I wanted to find out what sort of cell this is. It is blue, 65mm long, ~18.2mm in diameter and the sticker on the swatter says "1000mAh". Going by this [wikipedia article](https://en.wikipedia.org/wiki/List_of_battery_sizes#Cylindrical_lithium-ion_rechargeable_battery), I assume this to be a [18650 cell](https://en.wikipedia.org/wiki/18650_battery). The nominal voltage should then be more like 3.6-3.7 V and the capacity 1300-3500 mAh.

The insides of the battery handle are unspectecular, but do hold some space for potantial mods.
![Battery handle insides](/readme_src/battery_handle_insides.png)

For dissasembly of the main part, a ring around the female threads must be removed. I was able to carefully slide an old credit card in the gap and work it off.
![Credit card in ring retainer](/readme_src/credit_card_in_ring_retainer.png)

Next, the credit card method can be applied to the main housing.
![Credit card in main housing](/readme_src/credit_card_in_main_housing.png)

This is the view after opening.
![Videw after opening main housing](/readme_src/open_main_housing.png)

Two small phillips screws connect the boards to the housing.
![Highlited closeup of the two screws holding the boards](/readme_src/board_screws.png)

The boards can then be carefully lifted and laid out.
![Boards lifted up](/readme_src/boards_lifted.jpg)
![Boards untangeled](/readme_src/boards_untangeled.png)

Here is a closeup of the USB-power delivery board.
![USB board](/readme_src/usb-board.png)
Maybe this could be upgraded to USB-C

And the main board.
![Main Board](/readme_src/main_board.png)

Three wires go from the main board to the mesh, one red and two blue. They are directly connected to the large orange-red capacitor on the main board. The blue ground wires seem to be going to the outer mesh layers, and the positive red wire to the middle layer.
![Wires between main board and mesh](/readme_src/wires_main_board_to_mesh.png)

Here is a closeup of the writing on the capacitor
![Writing on capacitor](/readme_src/writing_on_capacitor.png)
I read:
```
CBB81
2KV 223J
```

My interpretation is:
CBB: Metallized polypropylene film capacitor (a type of film capacitor)
81: Specific model (built for high votlage pulses)

2KV: Rated voltage of 2000 V DC
223J: Capacitor code: 22 * 10^3 pF with a tolerance of J = +-5%, so roughly 22,000 pF, or 22 nF

A measurement with my multimeter to confirm the capacitance.
![Capacitance measurement](/readme_src/capacitance_measurement.png)
![Capacitance closeup](/readme_src/capacitance_closeup.png)
In fact the measured 23.269 nF are beyond the nominal tolerace (+5.8%), which could mean that this specific fly swatter does in fact have more oomph than others of the same model.

(Btw, if you have correctly bridged the mesh layers as shown in the beginning of the teardown, your meter should see an open loop)

Theoretically this capacitor could be replaced with one with even more capacitance. I have not researched the implications for the rest of the circuit tho. Also this could turn this relatively dangerous device into a super dangerous device.

In any case it seems relatively easy to swap in case it breaks, which I would assume to be a common failure mode.

## Plan

In order to "count" successfull hits of a conductive object with the swatter, I need to be able to sense discharges of the capacitor. For that, I could

- Directly measure the output of the capacitor to see when it drops.
- Measure the input of the capacitor, to see when it rises. 
- Find some other part of the circuit

It might be possible to pick up the 2 KV "zap" electromagnetically from a nearby wire coil. I could feed this coil into the analog input af a microcontroller and handle the rest from there without any modifications to the existing circuit. 