# The Environmental Impact of Cyberattacks: A Study of System Resource Utilization and Energy Consumption

> L’impact environnemental des cyberattaques : Étude de la sollicitation des ressources système et de la consommation énergétique

[![Thesis](https://img.shields.io/badge/Thesis-UQAC%20Constellation-6B8915)](https://constellation.uqac.ca/id/eprint/10491/)

## Abstract

### French Original

Alors que la recherche s’est longtemps concentrée sur les dimensions techniques et économiques de la cybersécurité, l’impact environnemental des activités cybercriminelles reste largement méconnu. Cette étude propose un cadre méthodologique novateur permettant de quantifier les émissions de gaz à effet de serre associées à trois vecteurs d’attaque majeurs : le cryptojacking, les rançongiciels (ransomwares) et les attaques par Déni de service (DoS). En se concentrant exclusivement sur les émissions opérationnelles, nos analyses montrent que la cybercriminalité représente une source significative de dégradation environnementale. Les résultats indiquent qu’en 2023, la consommation énergétique directe liée à ces attaques a généré une empreinte comprise entre 526 et 1052 MtCO2eq. Dans le scénario le plus élevé, ce total équivaut à 20 % des émissions annuelles des États-Unis ou à la moitié de l’empreinte du secteur mondial des Technologies de l’Information et de la Communication (TIC). Même dans son estimation la plus basse, cette empreinte dépasse celles d’États tels que la France, le Royaume-Uni ou l’Italie. Ces constats soulignent l’urgence d’intégrer la durabilité environnementale aux stratégies nationales et internationales de cybersécurité. Ce travail jette ainsi les bases d’une approche visant à faire de la lutte contre la cybercriminalité un levier essentiel de la transition écologique mondiale.

### English Translation

While research has long focused on the technical and economic dimensions of cybersecurity, the environmental impact of cybercriminal activities remains largely overlooked. This study proposes an innovative methodological framework to quantify the greenhouse gas emissions associated with three major attack vectors: cryptojacking, ransomware, and Denial-of-Service (DoS) attacks. Focusing exclusively on operational emissions, our analysis demonstrates that cybercrime represents a significant source of environmental degradation. The results indicate that in 2023, direct energy consumption from these attacks generated a footprint estimated between 526 and 1,052 MtCO2eq. In the upper-bound scenario, this total equals 20% of the annual emissions of the United States or half the footprint of the global Information and Communication Technology (ICT) sector. Even in its lower-bound estimate, this footprint exceeds that of countries such as France, the United Kingdom, or Italy. These findings highlight the urgent need to integrate environmental sustainability into national and international cybersecurity strategies. This work thus lays the groundwork for an approach aimed at turning the fight against cybercrime into a key lever for the global ecological transition.

## Data Architecture

The data processing pipeline is implemented in [notebook.ipynb](/notebook.ipynb) and processes heterogeneous threat intelligence logs and physical telemetry through a structured, multi-stage ETL and empirical analysis workflow.

| Data                           | Processing Phase           | Description                                                                                                                                                                      |
| ------------------------------ | -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [raw](/data/raw)               | Data Acquisition           | Original multi-format files (`.csv`, `.html`, `.log`) containing unparsed telemetry from measurement tools (ACS712, nmon, Open Hardware Monitor, Prometheus, System logs).       |
| [cleaned](/data/cleaned)       | Sanitation & Normalization | Standardized `.csv` datasets with encoding issues, mojibake characters, timestamp variations, and non-numeric artifacts sanitized.                                               |
| [featured](/data/featured)     | Variable Engineering       | Processed data augmented with calculated metrics (e.g., relative runtime, converted units, CPU/Memory percentages, and assigned attack modes: *Idle*, *Attack*, *Recovery*).     |
| [merged](/data/merged)         | Integration                | Unified dataset ([merged.csv](/data/merged/merged.csv)) combining featured logs across all attack vectors, host systems, measurement mechanisms, and execution runs.             |
| [aggregated](/data/aggregated) | Analytical Output          | Summarized mean metrics ([means.csv](/data/aggregated/means.csv)) grouped by attack category, host system, and attack mode for downstream energy and carbon accounting analysis. |

## Experimental Variables

The framework evaluates system behavior across **15 host environments**, **33 target attack configurations**, and **3 execution modes** (*Idle*, *Attack*, *Recovery*).

### Host Environments

- **Edge & Embedded Devices:** `Raspberry Pi 3B 1GB`, `Raspberry Pi 4B 2GB`, `Raspberry Pi 4B 4GB`, `Raspberry Pi 4B 8GB`, and `Raspberry Pi 5`.
- **Smart & IoT Equipment:** `HOSAFE HX-2PT1` (IP Camera), `D-Link DIR-822` (Router), `Huawei H151-381` (Router), and `TP-Link Tapo C200` (Smart Camera).
- **Workstations & Laptops:** `ASUS Zenbook UX51VZ`, `Apple M1 Pro`, `Apple MacBook Pro i7 2013`, `HP Victus 16-d0417nf`, and `Windows 11 i7 GeForce RTX 4060 Laptop`.
- **Virtualization:** `Windows 10 VM`.

### Threat Vectors & Attack Categories

- **Cryptojacking:** CPU/GPU miners including `CoinIMP`, `XMRig`, `GMiner`, `lolMiner`, `miniZ`, `NBMiner`, `NiceHash`, `OneZeroMiner`, `SRBMiner Multi`, `T-Rex`, and `WildRig Multi`.
- **Denial of Service (DoS):** Volumetric and application-layer attacks including `GoldenEye`, `Goloris`, `HULK`, `Hping3`, `MHDDoS (ICMP/TCP/UDP)`, `PyFlooder`, and `Slowloris`.
- **Ransomware:** Encryption payloads including `Bstry`, `Jigsaw`, `Petya`, `Randomware`, `Ransomware-PoC`, `Rex`, `Thanos`, and `WannaCry`.

## Dataset Summary Statistics

The table below provides key descriptive statistics ($\mu, \sigma$, range, quartiles).

|                                | count | mean ($\mu$) | std ($\sigma$) |       min |      25% |      50% |       75% |         max |
| :----------------------------- | ----: | -----------: | -------------: | --------: | -------: | -------: | --------: | ----------: |
| CPU Usage (%)                  | 12403 |      14.4192 |        21.0176 |         0 |      0.2 |      5.6 |        25 |         100 |
| CPU Temperature (°C)           | 15797 |      58.1456 |        13.6175 |        37 |       46 |     55.5 |        69 |          97 |
| Relative Time (s)              | 28387 |      557.071 |        586.251 |         0 |      173 |      366 |   660.708 |        3889 |
| Run                            | 28387 |      1.09596 |       0.368213 |         1 |        1 |        1 |         1 |           3 |
| Power (W)                      |  9016 |      3.21347 |        5.46682 |    -2.448 |    1.845 |     2.64 |     3.265 |        65.4 |
| Memory Usage (%)               | 12401 |      32.3301 |        26.3863 |    2.6176 |  7.07647 |     21.8 |      57.8 |         100 |
| Voltage (V)                    |  2788 |      2.40687 |      0.0559863 |      2.22 |     2.38 |      2.4 |      2.45 |        2.55 |
| Voltage Adjusted (V)           |   901 |    0.0572919 |      0.0340391 |     -0.01 |     0.03 |     0.05 |       0.1 |        0.13 |
| Current (A)                    |  8797 |     0.478125 |       0.295897 |    -0.204 |    0.191 |    0.514 |     0.633 |         1.5 |
| CPU Temperature Core #1 (°C)   |  9788 |      58.2444 |         14.561 |        34 |       44 |       58 |        73 |          97 |
| CPU Temperature Core #2 (°C)   |  9788 |      59.1924 |        15.1905 |        37 |       44 |       59 |        75 |          94 |
| CPU Temperature Core #3 (°C)   |  9788 |      57.3792 |        14.4259 |        35 |       43 |       55 |        72 |          92 |
| CPU Temperature Core #4 (°C)   |  9788 |      57.6006 |        14.2273 |        35 |       44 |       57 |        73 |          92 |
| CPU Power (W)                  |  9788 |      12.4929 |        7.14874 |       2.8 |      5.2 |     13.5 |      17.5 |        52.3 |
| CPU Power Cores (W)            |  9788 |      8.81229 |         6.4361 |       0.4 |      1.8 |      9.6 |      13.5 |        45.9 |
| CPU Graphics Power (W)         |  9788 |    0.0746731 |        0.42849 |         0 |        0 |        0 |         0 |        10.6 |
| GPU Temperature (°C)           |  9788 |      12.3266 |         24.256 |         0 |        0 |        0 |         0 |          96 |
| GPU Usage (%)                  |  4022 |      28.8241 |          44.54 |         0 |        0 |        0 |        95 |         100 |
| GPU Memory Usage (%)           |  4018 |      28.3296 |         38.057 |         2 |      2.4 |      2.4 |      66.9 |        99.6 |
| GPU Power (W)                  |     2 |        26.25 |         22.981 |        10 |   18.125 |    26.25 |    34.375 |        42.5 |
| CPU Usage User (%)             |  3600 |      3.24144 |        10.7004 |         0 |        0 |        0 |       0.2 |        96.5 |
| CPU Usage System (%)           |  3600 |      5.30283 |        11.2813 |         0 |        0 |      0.2 |       0.5 |        43.1 |
| CPU Usage Wait (%)             |  3600 |     0.120556 |         2.3687 |         0 |        0 |        0 |         0 |        72.7 |
| Processes Runnable             |  3600 |      1.44972 |        1.38412 |         1 |        1 |        1 |         1 |          13 |
| Processes Blocked              |  3600 |    0.0172222 |       0.325337 |         0 |        0 |        0 |         0 |          13 |
| Process Switches               |  3600 |      7798.27 |        17536.3 |         0 |     45.9 |     89.8 |    253.25 |      100113 |
| Process Forks                  |  3600 |      0.72775 |        1.16717 |         0 |        0 |        0 |         2 |        27.9 |
| Process Execs                  |  3600 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Network lo Received (Ko/s)     |  3600 |     0.183528 |        2.71176 |         0 |        0 |        0 |         0 |          52 |
| Network eth0 Received (Ko/s)   |  3600 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Network wlan0 Received (Ko/s)  |  3600 |      13.3728 |        56.4516 |         0 |        0 |        0 |         0 |       455.2 |
| Network lo Sent (Ko/s)         |  3600 |    -0.183528 |        2.71176 |       -52 |       -0 |       -0 |         0 |          -0 |
| Network eth0 Sent (Ko/s)       |  3600 |            0 |              0 |        -0 |       -0 |       -0 |         0 |          -0 |
| Network wlan0 Sent (Ko/s)      |  3600 |      -1317.1 |        3753.11 |  -13840.9 |       -0 |       -0 |         0 |          -0 |
| Network Packet lo Received     |  3600 |     0.329306 |        4.82705 |         0 |        0 |        0 |         0 |        92.8 |
| Network Packet eth0 Received   |  3600 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Network Packet wlan0 Received  |  3600 |      77.0146 |        239.447 |         0 |        0 |        0 |         0 |      1483.8 |
| Network Packet lo Sent         |  3600 |     0.329306 |        4.82705 |         0 |        0 |        0 |         0 |        92.8 |
| Network Packet eth0 Sent       |  3600 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Network Packet wlan0 Sent      |  3600 |      1522.26 |        4762.35 |         0 |        0 |        0 |         1 |     21423.1 |
| Disk mmcblk0 Usage (%)         |  1200 |       0.5865 |         5.8871 |         0 |        0 |        0 |         0 |         101 |
| Disk mmcblk0p1 Usage (%)       |  1200 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Disk mmcblk0p2 Usage (%)       |  1200 |         0.59 |        5.88841 |         0 |        0 |        0 |         0 |         101 |
| Disk mmcblk0 Read (Ko/s)       |  1200 |      326.362 |        5884.58 |         0 |        0 |        0 |         0 |      138943 |
| Disk mmcblk0p1 Read (Ko/s)     |  1200 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Disk mmcblk0p2 Read (Ko/s)     |  1200 |      326.362 |        5884.58 |         0 |        0 |        0 |         0 |      138943 |
| Disk mmcblk0 Write (Ko/s)      |  1200 |      8.37017 |         18.186 |         0 |        0 |        0 |        12 |       159.7 |
| Disk mmcblk0p1 Write (Ko/s)    |  1200 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Disk mmcblk0p2 Write (Ko/s)    |  1200 |      8.37017 |         18.186 |         0 |        0 |        0 |        12 |       159.7 |
| Disk mmcblk0 Block Size (Ko)   |  1200 |      3.45192 |        8.87437 |         0 |        0 |        0 |         6 |       106.4 |
| Disk mmcblk0p1 Block Size (Ko) |  1200 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Disk mmcblk0p2 Block Size (Ko) |  1200 |      3.45192 |        8.87437 |         0 |        0 |        0 |         6 |       106.4 |
| Disk mmcblk0 Transfers         |  1200 |      5.01858 |        66.3251 |         0 |        0 |        0 |         2 |        1489 |
| Disk mmcblk0p1 Transfers       |  1200 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Disk mmcblk0p2 Transfers       |  1200 |      5.01858 |        66.3251 |         0 |        0 |        0 |         2 |        1489 |
| JFS dev Usage (%)              |  3600 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| JFS run Usage (%)              |  3600 |          0.2 |              0 |       0.2 |      0.2 |      0.2 |       0.2 |         0.2 |
| JFS Usage (%)                  |  3600 |          7.6 |        1.00513 |       5.5 |      7.7 |      7.7 |       8.5 |         8.5 |
| JFS boot/firmware Usage (%)    |  3600 |           28 |              0 |        28 |       28 |       28 |        28 |          28 |
| Memory Active (%)              |  3600 |      1.91783 |         1.1942 | 0.0211238 |  1.27799 |  1.60409 |   1.85361 |     4.45976 |
| Memory Buffers (%)             |  3600 |     0.459901 |       0.296008 |         0 | 0.311576 | 0.359104 |  0.396071 |     1.11164 |
| Memory Cached (%)              |  3600 |      5.93018 |        7.12886 |  0.142586 |  2.20744 |  3.14876 |   3.84981 |     21.7522 |
| Memory Inactive (%)            |  3600 |      5.98853 |        9.09347 |   1.05883 |  1.95923 |  2.97317 |   3.76267 |     96.3377 |
| Disk mmcblk1 Usage (%)         |  2400 |     0.165208 |       0.519621 |         0 |        0 |        0 |         0 |           9 |
| Disk mmcblk1p1 Usage (%)       |  2400 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Disk mmcblk1p2 Usage (%)       |  2400 |     0.166458 |       0.516404 |         0 |        0 |        0 |         0 |           9 |
| Disk mmcblk1 Read (Ko/s)       |  2400 |      2.30571 |        76.9588 |         0 |        0 |        0 |         0 |      3359.3 |
| Disk mmcblk1p1 Read (Ko/s)     |  2400 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Disk mmcblk1p2 Read (Ko/s)     |  2400 |      2.30571 |        76.9588 |         0 |        0 |        0 |         0 |      3359.3 |
| Disk mmcblk1 Write (Ko/s)      |  2400 |      7.32183 |        17.5445 |         0 |        0 |        0 |         8 |       283.3 |
| Disk mmcblk1p1 Write (Ko/s)    |  2400 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Disk mmcblk1p2 Write (Ko/s)    |  2400 |      7.32183 |        17.5464 |         0 |        0 |        0 |         8 |       283.3 |
| Disk mmcblk1 Block Size (Ko)   |  2400 |      2.94154 |        5.60268 |         0 |        0 |        0 |         6 |        88.8 |
| Disk mmcblk1p1 Block Size (Ko) |  2400 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Disk mmcblk1p2 Block Size (Ko) |  2400 |      2.93654 |        5.59518 |         0 |        0 |        0 |         6 |        88.8 |
| Disk mmcblk1 Transfers         |  2400 |     0.912167 |         2.5383 |         0 |        0 |        0 |         1 |        37.9 |
| Disk mmcblk1p1 Transfers       |  2400 |            0 |              0 |         0 |        0 |        0 |         0 |           0 |
| Disk mmcblk1p2 Transfers       |  2400 |     0.912167 |        2.53846 |         0 |        0 |        0 |         1 |        37.9 |
| Network (Ko/s)                 |  1992 |      2.40708 |        15.4457 |         0 |        0 |        0 | 0.0878906 |     536.861 |
| Disk Read (Mo/s)               |   906 |  4.41501e-05 |    0.000813039 |         0 |        0 |        0 |         0 |        0.02 |
| Disk Write (Mo/s)              |   906 |     0.462196 |        2.28141 |         0 |        0 |        0 |      0.01 |        26.5 |
| Encrypted Files                |   906 |      487.507 |        555.743 |         0 |        0 |      144 |       802 |        1432 |
| Encrypted Disk Usage (%)       |   906 |      30.1977 |        35.9688 |         0 |        0 |   18.535 |     39.27 |        96.9 |
| Encrypted Files Size (Mo)      |   682 |      44.2062 |        74.2018 |         0 |        0 |      1.3 |     73.86 |       193.7 |
| Encryption Speed (o/s)         |   906 |       452105 |    3.04599e+06 |         0 |        0 |        0 |         0 | 5.83715e+07 |

> *Table generated automatically using pandas:* `merged_df.describe().T.to_markdown()`

## Statistical Analysis

To evaluate whether system resource consumption and physical power draw increase significantly during active cyberattacks compared to baseline idle states, paired non-parametric Wilcoxon signed-rank tests were performed across all recorded telemetry metrics within each attack vector.

|      | Metric                         | Attack Type       |    n | Wilcoxon Statistic |     p-value | Significant |
| ---: | :----------------------------- | :---------------- | ---: | -----------------: | ----------: | :---------- |
|    0 | CPU Usage (%)                  | Cryptojacking     |   19 |                179 | 0.000104904 | True        |
|    1 | CPU Usage (%)                  | Denial of Service |   10 |                 51 |  0.00683594 | True        |
|    2 | CPU Usage (%)                  | Ransomware        |   14 |                105 | 0.000478119 | True        |
|    3 | CPU Temperature (°C)           | Cryptojacking     |   23 |                276 | 1.19209e-07 | True        |
|    4 | CPU Temperature (°C)           | Denial of Service |    6 |                 19 |    0.046875 | True        |
|    5 | CPU Temperature (°C)           | Ransomware        |   14 |                105 | 0.000489353 | True        |
|    6 | Power (W)                      | Cryptojacking     |    2 |                  3 |        0.25 | False       |
|    7 | Power (W)                      | Denial of Service |   17 |                152 |  0.00017543 | True        |
|    8 | Power (W)                      | Ransomware        |   14 |                105 | 0.000486528 | True        |
|    9 | Memory Usage (%)               | Cryptojacking     |   18 |                171 |  3.8147e-06 | True        |
|   10 | Memory Usage (%)               | Denial of Service |   10 |               53.5 |  0.00292969 | True        |
|   11 | Memory Usage (%)               | Ransomware        |   14 |                104 | 0.000592249 | True        |
|   12 | Voltage (V)                    | Cryptojacking     |    1 |                  1 |         0.5 | False       |
|   13 | Voltage (V)                    | Denial of Service |    3 |                  0 |           1 | False       |
|   14 | Voltage (V)                    | Ransomware        |    4 |                  0 |           1 | False       |
|   15 | Voltage Adjusted (V)           | Cryptojacking     |    1 |                  1 |         0.5 | False       |
|   16 | Current (A)                    | Cryptojacking     |    1 |                  1 |         0.5 | False       |
|   17 | Current (A)                    | Denial of Service |   13 |                 55 | 0.000976562 | True        |
|   18 | Current (A)                    | Ransomware        |    4 |                 10 |      0.0625 | False       |
|   19 | CPU Temperature Core #1 (°C)   | Cryptojacking     |   15 |                120 | 3.05176e-05 | True        |
|   20 | CPU Temperature Core #2 (°C)   | Cryptojacking     |   15 |                120 | 3.05176e-05 | True        |
|   21 | CPU Temperature Core #3 (°C)   | Cryptojacking     |   15 |                120 | 3.05176e-05 | True        |
|   22 | CPU Temperature Core #4 (°C)   | Cryptojacking     |   15 |                120 | 3.05176e-05 | True        |
|   23 | CPU Power (W)                  | Cryptojacking     |   15 |                111 |  0.00100708 | True        |
|   24 | CPU Power Cores (W)            | Cryptojacking     |   15 |                107 |  0.00268555 | True        |
|   25 | CPU Graphics Power (W)         | Cryptojacking     |   15 |                 45 |    0.142763 | False       |
|   26 | GPU Temperature (°C)           | Cryptojacking     |   15 |                103 |  0.00622559 | True        |
|   27 | GPU Usage (%)                  | Cryptojacking     |   11 |                 66 | 0.000488281 | True        |
|   28 | GPU Memory Usage (%)           | Cryptojacking     |    9 |                 45 |  0.00195312 | True        |
|   29 | GPU Power (W)                  | Cryptojacking     |    1 |                  1 |         0.5 | False       |
|   30 | CPU Usage User (%)             | Denial of Service |    6 |                 20 |     0.03125 | True        |
|   31 | CPU Usage System (%)           | Denial of Service |    6 |                 20 |     0.03125 | True        |
|   32 | CPU Usage Wait (%)             | Denial of Service |    6 |                  5 |     0.78125 | False       |
|   33 | Processes Runnable             | Denial of Service |    6 |                 21 |    0.015625 | True        |
|   34 | Processes Blocked              | Denial of Service |    3 |                  6 |       0.125 | False       |
|   35 | Process Switches               | Denial of Service |    6 |                 20 |     0.03125 | True        |
|   36 | Process Forks                  | Denial of Service |    6 |               13.5 |    0.296875 | False       |
|   37 | Network lo Received (Ko/s)     | Denial of Service |    1 |                  1 |         0.5 | False       |
|   38 | Network wlan0 Received (Ko/s)  | Denial of Service |    6 |                 15 |     0.03125 | True        |
|   39 | Network lo Sent (Ko/s)         | Denial of Service |    1 |                  0 |           1 | False       |
|   40 | Network wlan0 Sent (Ko/s)      | Denial of Service |    5 |                  0 |           1 | False       |
|   41 | Network Packet lo Received     | Denial of Service |    1 |                  1 |         0.5 | False       |
|   42 | Network Packet wlan0 Received  | Denial of Service |    6 |                 20 |     0.03125 | True        |
|   43 | Network Packet lo Sent         | Denial of Service |    1 |                  1 |         0.5 | False       |
|   44 | Network Packet wlan0 Sent      | Denial of Service |    6 |                 15 |     0.03125 | True        |
|   45 | Disk mmcblk0 Usage (%)         | Denial of Service |    2 |                  2 |         0.5 | False       |
|   46 | Disk mmcblk0p2 Usage (%)       | Denial of Service |    2 |                  2 |         0.5 | False       |
|   47 | Disk mmcblk0 Read (Ko/s)       | Denial of Service |    2 |                  2 |         0.5 | False       |
|   48 | Disk mmcblk0p2 Read (Ko/s)     | Denial of Service |    2 |                  2 |         0.5 | False       |
|   49 | Disk mmcblk0 Write (Ko/s)      | Denial of Service |    2 |                  0 |           1 | False       |
|   50 | Disk mmcblk0p2 Write (Ko/s)    | Denial of Service |    2 |                  0 |           1 | False       |
|   51 | Disk mmcblk0 Block Size (Ko)   | Denial of Service |    2 |                  2 |         0.5 | False       |
|   52 | Disk mmcblk0p2 Block Size (Ko) | Denial of Service |    2 |                  2 |         0.5 | False       |
|   53 | Disk mmcblk0 Transfers         | Denial of Service |    2 |                  2 |         0.5 | False       |
|   54 | Disk mmcblk0p2 Transfers       | Denial of Service |    2 |                  2 |         0.5 | False       |
|   55 | JFS run Usage (%)              | Denial of Service |    6 |                  0 |           1 | False       |
|   56 | JFS Usage (%)                  | Denial of Service |    6 |                  0 |           1 | False       |
|   57 | JFS boot/firmware Usage (%)    | Denial of Service |    6 |                  0 |           1 | False       |
|   58 | Memory Active (%)              | Denial of Service |    6 |                  3 |       0.625 | False       |
|   59 | Memory Buffers (%)             | Denial of Service |    6 |                 11 |        0.25 | False       |
|   60 | Memory Cached (%)              | Denial of Service |    6 |                6.5 |       0.375 | False       |
|   61 | Memory Inactive (%)            | Denial of Service |    6 |                 21 |    0.015625 | True        |
|   62 | Disk mmcblk1 Usage (%)         | Denial of Service |    4 |                  5 |      0.5625 | False       |
|   63 | Disk mmcblk1p2 Usage (%)       | Denial of Service |    4 |                  5 |      0.5625 | False       |
|   64 | Disk mmcblk1 Read (Ko/s)       | Denial of Service |    3 |                  1 |       0.875 | False       |
|   65 | Disk mmcblk1p2 Read (Ko/s)     | Denial of Service |    3 |                  1 |       0.875 | False       |
|   66 | Disk mmcblk1 Write (Ko/s)      | Denial of Service |    4 |                  4 |      0.6875 | False       |
|   67 | Disk mmcblk1p2 Write (Ko/s)    | Denial of Service |    4 |                  4 |      0.6875 | False       |
|   68 | Disk mmcblk1 Block Size (Ko)   | Denial of Service |    4 |                  3 |      0.8125 | False       |
|   69 | Disk mmcblk1p2 Block Size (Ko) | Denial of Service |    4 |                  3 |      0.8125 | False       |
|   70 | Disk mmcblk1 Transfers         | Denial of Service |    4 |                  4 |      0.6875 | False       |
|   71 | Disk mmcblk1p2 Transfers       | Denial of Service |    4 |                  4 |      0.6875 | False       |
|   72 | Network (Ko/s)                 | Denial of Service |    3 |                  6 |       0.125 | False       |
|   73 | Disk Write (Mo/s)              | Ransomware        |    4 |                 10 |      0.0625 | False       |
|   74 | Encrypted Files                | Ransomware        |    4 |                 10 |      0.0625 | False       |
|   75 | Encrypted Disk Usage (%)       | Ransomware        |    4 |                 10 |      0.0625 | False       |
|   76 | Encrypted Files Size (Mo)      | Ransomware        |    3 |                  6 |       0.125 | False       |
|   77 | Encryption Speed (o/s)         | Ransomware        |    4 |                 10 |      0.0625 | False       |

> *Table generated automatically using pandas:* `statistics_df.to_markdown()`
