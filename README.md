# Dataset
This repo contains timing prediction dataset download instructions and documentation.

This dataset is made from open-source PDK [skywater130](https://github.com/google/skywater-pdk) and open-source EDA flow [OpenROAD](https://github.com/The-OpenROAD-Project). It contains 21 real-world benchmark circuits:

| Benchmark      | #Nodes | #Net Edges | #Cell Edges | #Endpoints |
| -------------- | ------ | ---------- | ----------- | ---------- |
| blabla         | 55568  | 39853      | 35689       | 1614       |
| usb\_cdc\_core | 7406   | 5200       | 4869        | 630        |
| BM64           | 38458  | 27843      | 25334       | 1800       |
| salsa20        | 78486  | 57737      | 52895       | 3710       |
| aes128         | 211045 | 148997     | 138457      | 5696       |
| wbqspiflash    | 9672   | 6798       | 6454        | 323        |
| cic\_decimator | 3131   | 2232       | 2102        | 130        |
| aes256         | 290955 | 207414     | 189262      | 11200      |
| des            | 60541  | 44478      | 41845       | 2048       |
| aes\_cipher    | 59777  | 42671      | 41411       | 660        |
| picorv32a      | 58676  | 43047      | 40208       | 1920       |
| zipdiv         | 4398   | 3102       | 2913        | 181        |
| genericfir     | 38827  | 28845      | 25013       | 3811       |
| usb            | 3361   | 2406       | 2189        | 344        |
| jpeg\_encoder  | 238216 | 176737     | 167960      | 4422       |
| usbf\_device   | 66345  | 46241      | 42226       | 4404       |
| aes192         | 234211 | 165350     | 152910      | 8096       |
| xtea           | 10213  | 7151       | 6882        | 423        |
| spm            | 1121   | 765        | 700         | 129        |
| y\_huff        | 48216  | 33689      | 30612       | 2391       |
| synth\_ram     | 25910  | 19024      | 16782       | 2112       |

In [our DAC22 work](https://guozz.cn/publication/mltimerdac-22/), the upper 14 benchmarks are used for training and the lower 7 are used for testing. (We used a [modified OpenSTA](https://github.com/TimingPredict/OpenSTADump) to dump timing-sensitive intermediate data. Please see our paper for other details on the benchmark and settings.)



## Raw Netlist Download

[*New 2023/6/7*] You can now download our raw design data from one of the following links: (7-zipped ~500MB)

https://disk.pku.edu.cn:443/link/90D8E40611678D1A24C214A1EFBA9630

https://drive.google.com/file/d/1QimU8q2cIADLBVL6GGFRm6tSBKbj3ZbB/view?usp=sharing

https://cloud.guozz.cn/s/x9T4

The archive contains:

* The [skywater130](https://github.com/google/skywater-pdk/) PDK (in `./techlib`), including liberty and lef files.
* The gate-level Verilog, DEF, SDC, SPEF, and SDF files for all the above 21 circuits, implemented using [OpenROAD](https://github.com/The-OpenROAD-Project/OpenROAD).

Have fun hacking!



## Graph Data Download for GNN Training & Inference
Please choose one of the links below. The 7-zipped file is about 200 MB in size.

https://disk.pku.edu.cn:443/link/A01D052FB4A134A1523AD101F7F5B511

https://drive.google.com/file/d/1kknTAi8x55bgFeHb8UVnVUpw3cCvkZMe/view?usp=sharing

https://cloud.guozz.cn/s/mOsK



## Documentation
You can also refer to our [code](https://github.com/TimingPredict/TimingPredict) for usage.

### `8_rat`: Full annotated timing graph dataset
This contains dumpped [DGL](https://www.dgl.ai/) heterogeneous graphs.

There are three kinds of edges and one kind of node.
[Here](https://github.com/TimingPredict/Dataset/issues/1) is an explanation to the meaning of edge types.
The information included is:

#### `net_out` and `net_in`
net arcs.

Features:

- `2x`: relative position

#### `cell_out`
cell arcs.

Features:

-   `2*4*(1+7+7)x`: [E/L]\* cell\_{rise, fall}, {rise, fall}\_transition {is\_valid, xindex, yindex}
-   `2*4*49x`: [E/L]\* cell\_{rise, fall}, {rise, fall}\_transition values
-   `4x`: cell delay annotations (EL/RF)

#### Node
Features:

*   `1x`: is primary I/O pin (1) or not (0)
*   `1x`: is fanin (0) or fanout (1)
*   `4x`: relative to the top/left/right/bottom of die area
*   `4x`: capacitance information (EL/RF) in cell library
*   `4x`: net delay annotations (EL/RF) for fanin pins
*   `4x`: arrival time annotations (EL/RF)
*   `4x`: slew annotations (EL/RF)
*   `1x`: is timing endpoint (i.e. has constraint) (1) or not (0)
*   `4x`: required arrival time annotations (EL/RF)

#### Usage
One can use the cell delay annotations, node slew/at/netdelay/rat annotations as tasks, and leave other features as model inputs.

### `4_netstat`: The statistics-based net delay dataset
(To be filled here.)

### `7_homotest`: The simple homograph dataset
(To be filled here.) This is not intended to be used.




## Reference
Please cite our work if you find this dataset useful.

``` tex
@inproceedings{mltimerdac22,
 author = {Guo, Zizheng and Liu, Mingjie and Gu, Jiaqi and Zhang, Shuhan and Pan, David Z. and Lin, Yibo},
 booktitle = {Proceedings of the 59th Annual Design Automation Conference 2022},
 organization = {ACM},
 title = {A Timing Engine Inspired Graph Neural Network Model for Pre-Routing Slack Prediction},
 year = {2022}
}
```

