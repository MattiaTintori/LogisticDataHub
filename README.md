# Logistic Data Hub
This project is dedicated to the data-driven optimization of warehouse locations. This repository aims to determine the most efficient placement of one or more warehouses, ensuring minimal costs.

## Table of Contents

- [Introduction](#introduction)
- [Objective](#objective)
- [Methodology](#methodology)
  - [1. Dataset and Data Cleaning](#1-dataset-and-data-cleaning)
  - [2. Exploratory Analysis](#2-exploratory-analysis)
  - [3. Clustering](#3-clustering)
  - [4. Additional Datasets](#4-additional-datasets)
  - [5. Optimization](#5-optimization)
- [Results](#results)
- [Future Directions](#future-directions)

## Introduction

The aim of this project is to assist in the strategic location selection of logistic hubs, utilizing a data-driven approach. It integrates various datasets and employs advanced optimization techniques to provide actionable insights for warehouse location decisions.

## Objective

The primary objective is to identify optimal locations in the Lombardy region for one or more logistic hubs. This involves using a data-driven strategy to balance various factors such as costs, geographic distribution, and logistical efficiency.

## Methodology

### 1. Dataset and Data Cleaning

We utilized a real anonymized dataset containing:
- Customer codes
- Customer locations
- Provinces
- Geographic coordinates
- Order frequency over the last year

Data inconsistencies between city/province designations and geographic coordinates were corrected by referencing validated geographic datasets. Gaps in the data were addressed by performing reverse geocoding on the coordinates, utilizing a locally hosted Docker container running an Open Source Routing Machine (OSRM) image.


### 2. Exploratory Analysis

Exploratory data analysis was performed to identify patterns and insights within the dataset. Key findings were used to guide the clustering and optimization stages.
<div class="image-grid">
    <img src="images/Ex_Analysis2.png" alt="Exploratory Analysis 1">
    <img src="images/Ex_Analysis1.png" alt="Exploratory Analysis 2">
</div>

### 3. Clustering

Clustering was performed in three steps:
1. **Initial Elbow Method Analysis**  
The graph represents the elbow method applied to determine the optimal number of clusters in a K-means clustering algorithm. The x-axis denotes the number of clusters, while the y-axis represents the Within-Cluster Sum of Squares (WCSS), a metric that quantifies the variance within each cluster. The plot reveals a distinct "elbow" at three clusters, where the rate of WCSS reduction diminishes. This point offer the best balance between minimizing variance within clusters and avoiding overfitting.

![Elbow Method](images/Elbow.png)

2. **Original Clustering**  
Based on the identification of three optimal clusters from the K-means clustering analysis, we assigned each customer to one of these clusters according to their feature similarities and color coded the customers based on their cluster assignments.
3. **Modified Clustering**   
To ensure that all clusters were geographically constrained within the Lombardy region, we refined the clustering process by adjusting the cluster centroids. Specifically, the centroids were repositioned by calculating the nearest point within Lombardy for each centroid initially falling outside the region.

![Modified Clustering](images/Modified_Clusters.png)

### 4. Additional Datasets

To enrich the analysis, additional datasets were utilized:
- **Logistic Enterprises**  
Source: Open Data from Regione Lombardia
  
![Logistic Enterprises](images/Logistic_Companies.png)

- **Labor Availability**  
Source: Population data from ISTAT

![Workforce](images/Workforce.png)

- **Real Estate Values**  
Source: Data from Immobiliare.it.
### 5. Optimization

The optimization process involved:
- **Initial Clusters**: 3 clusters (Ponti sul Mincio, Palestro, Bollate).
- **Simulation**: 159 points were simulated across various provinces.
- **Decision Variables**: Fixed costs, using the average price per square meter by province, variable costs, using the distance and cost matrices, proximity to logistic enterprises and labor availability.

Transportation costs were calculated using the Haversine Distance  
a = sin²(Δφ / 2) + cos(φ₁) * cos(φ₂) * sin²(Δλ / 2) c = 2 * atan2(√a, √(1 - a)) d = R * c

where

- Δφ = φ₂ - φ₁ (difference in latitude)
- Δλ = λ₂ - λ₁ (difference in longitude)
- φ₁, φ₂ are the latitudes of the two points in radians
- λ₁, λ₂ are the longitudes of the two points in radians
- R is the Earth's radius (mean radius = 6,371 km)
- d is the distance between the two points

![Optimization Function](images/Mathematical_formula.png)

Optimization resulted in the selection of two warehouse locations:
- **Warehouse 1**: Casorate Primo, Pavia.
- **Warehouse 2**: Ponti sul Mincio, Mantova.

![Optimization](images/Warehouse_Location.png)

![Optimization](images/Warehouse_Distribution.png)

## Results

The optimization process identified two strategic locations for logistic hubs, covering approximately 63% of the customer base within a one-hour reach. These locations balance fixed and variable costs, labor availability, and proximity to logistic enterprises.

![Isochrone Map](images/Isochrone.png)

## Future Directions

Future improvements could include:
- Using road distances instead of straight-line distances.
- Incorporating rental costs as a variable.
- Considering other transportation modes besides highways.

## Contributors

- **Lorenzo Battisti**
- **Andrea Bonassi**
- **Luca Calzaferri**
- **Daniel Conte**
- **Mattia Piantoni**
- **Mattia Tintori**
