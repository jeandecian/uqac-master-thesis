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
