---
title: "Lecture 4: Dimensional Modelling for BI Applications"
created: '2019-08-25T03:49:29Z'
modified: '2019-09-16T04:23:03Z'
tags: [Notebooks/Computer Science Degree, Lectures]
---

# Lecture 4: Dimensional Modelling for BI Applications

## Agenda

1. Understanding what a multi-dimension model is
2. Conceptual dimensional modelling with 'Thomsen Diagrams'
3. Data Warehouses
## Multi-Dimensional Modelling: Designing data models for decision support

### What is Multi-Dimensional Data?

* The concept of "Information 'space'"
* The concept of "dimensions"
An approach to structuring a view of datathat provides an easy to understand and navigate database

* The aim is to encourage understanding, exploration and learning
* 'Subject'-oriented
### Multi-Dimensionality

* Each number has a set of associated attributes

    * What it measures
    * At what point of time it was created
    * What location it is from
    * What product it is associated with
    * What promotion it is associated with
Etc.


* Often talk about information spaces ascubes, hyper cubesorn-cubes.
### Dimensional Models

* Aone-dimensional informational space:
![Sales S,OOOS 
12 8 26 15 32 19 11 
2001 2002 2003 2004 2005 2006 2007 
Years ](@attachment/3f8312a4-4411-4044-bb07-dce0e8f37599.png)* Atwo-dimensional information space:
![Sales S,OOOS 
Products 
Hinges 
Screws 
Nuts 
Flanges 
Springs 
Sprockets 
Widgets 
9 
13 
42 
22 
10 
14 
16 
12 
11 
7 
10 
15 
11 
22 
33 
8 
26 
33 
15 
66 
12 
32 
29 
26 
18 
14 
9 
38 
15 
27 
9 
15 
14 
26 
25 
16 
22 
41 
13 
32 
41 
16 
21 
51 
24 
15 
12 
19 
39 
14 
25 
55 
28 
33 
7 
11 
2001 2002 2003 2004 2005 2006 2007 
Years ](@attachment/37f915ea-a5a2-4569-a571-b0a5d73ff69a.png)* Athree-dimensional information space:
![Sales S,OOOS 
9 11 26 18 14 41 39 
13 
7 33 14 26 16 14 
Products 
Hinges 
Screws 
Nuts 
Flanges 
Springs 
Sprockets 
Widgets 
22 
10 
14 
16 
12 
15 66 38 16 51 
11 
12 15 22 24 
22 32 27 41 
15 
33 29 9 
13 12 
8 
26 15 32 19 
55 
28 
33 
7 
11 
2001 2002 2003 2004 2005 2006 2007 
Years 
Locations ](@attachment/4a7adb84-3af9-42a2-8ee9-6215f08d4f65.png)### Extracting Data: Slicing and Dicing

![SalesS.OOOs 
Sales Nu $ , 罒 $ 
11 26 14 41 39 
12 15 22 24 28 
2 1 2002 2832 【 4 200520062 〕 7 
Springs 14 22 32 27 41 
Sprockets 16 33 9 13 12 7 
1 282 20032004 20052 ( 6 
Sa $ OTA Produc f8284 $ , 08 $ 
Nuts 
Springs 
Sprockets 
Melb Syd Adel Bris ](@attachment/1110086c-4d7b-459c-9d4c-77982582f094.png)### What is aDimension?

* Different for different parts of the BI system

    * A dimensional data warehouse structure vs. dimensional view presented to different users

* Is a design issue

    * They aren't 'natural'

* Should make sense to end users

    * Users understand when they see a detailed report based on dimensions

* Should we even use the term 'dimension' with end users?
## Dimensions, hierarchies and aggregates

### Design Constraints

* The 'Business'

    * The reports that are required

* The 'Data'

    * The data that is available

### Relational to Dimensional: A Mindset Shift

* Standard data modelling

    * Entities
    * Attributes
    * Relationships
    * Cardinalities

* Dimensional modelling

    * Facts
    * Measures
    * Dimensions
    * Members
    * Hierarchies

### Dimensional Hierarchy - example

![6u 三 29 一 01 
の u 一 8 一 0 ト 
! on で 0 」 d 
一 里 0 ト ](@attachment/ac03f95c-1553-444c-82ff-642aa4c4622d.png)### Dimensions with Multiple Hierarchies

![City 
Customer 
Calendar 
Year 
Fiscal 
Year 
State 
Sales 
Region 
Calendar 
Quarter 
Fiscal 
Quarter 
Sales 
District 
Calendar 
Month 
Fiscal 
Month 
Calendar 
Day 
Fiscal 
Week ](@attachment/8807f023-b11b-4f64-8259-e971699c449d.png)### Dimension Hierarchies

* Vital aspect of the design for 'drill-down'
* Important to identify additive and non-additive facts
* Each level in the hierarchy is an aggregate of the level below

    * Each time data at one level in the hierarchy is shown, the level below has to be processed (usually summed).
    * Some tools provide caching, but not all
    * Aggregates can be stored in the data warehouse if designed well.

Option 1:Calculate aggregates as needed

Option 2: Pre-calculate aggregates

### Modelling Issues

* Continuous vs. discrete
* Additive, semi-additive and non-additive
* Calculations run across dimensions and can cause conflict.
* Time dimension

    * There is always a time dimension.
    * Multiple hierarchies (e.g. calendar and financial years)

        * Multiple approaches to design.


* Some dimensions are large

    * E.g. a customer dimension for a large Telco - millions of members.

* Dimensions change over time

    * Customer addresses, product code, etc.
    * We cannot update records, only add -> need to cater for changes.

* Dimensions that aren't clean

    * Uneven, jagged, unlevelled.

## Conceptual modelling withThomsen Diagrams

### Multi-Dimensional Domain Structure

* Each dimension -> a vertical line

    * Each dimension is described independently

* Every member of a dimension -> a unit interval on the line
* A special 'dimension' for the measured facts.
* A multi-dimension model is built by combining the 'lines' for the involved dimensions.
### Example Time Dimension

![Time 
Jan 
Feb 
Mar 
Qtr 1 
Apr 
May 
Jun 
Qtr 2 
Jul 
Aug 
Sep 
Qtr 3 
Oct 
Nov 
Qtr 4 ](@attachment/7b16cf00-f9d1-41c7-b00b-cc13c4e78e1e.png)### A Simple Three Dimensional Model

![Time 
Product 
Data generating event 
Sales S 
Shoes 
March 
arch 
Measure 
ales 
Time 
S 
S hoes 
Product 
Thomsen 1997, p54 ](@attachment/bc10e901-531a-4e22-bf15-e34f35fd2fb0.png)### A Six Dimensional Model

![Time 
Store 
Customer 
Product 
Scenario 
Type 
Measures ](@attachment/d3dc0a7f-5a58-4bc7-921c-57d22282511c.png)### Conceptual Modelling

* Rather than showing every single value you show each type in the hierarchy
* Not easy to show multiple hierarchies
![Time 
Year (1) 
Quarter (4) 
Month (12) ](@attachment/8b14e9fb-688f-4158-9462-6ccc39405e07.png)#### An example of conceptual modelling

![Time 
Year (1) 
Quarter (4) 
Month (12) 
No of 
17 
levels: 
Store 
Total (I) 
State (2) 
Region (5) 
Store (28) 
Products 
Measures 
Total (1) 
Line (5) 
Brand (7) 
Product (56) 
Measures (12) 
36 
Total Data Items (Potential) — 
69 
12 
-17x36x69x 12 = 506,736 ](@attachment/dd4757cd-d692-4a96-8a3e-d309fa1b3f39.png)#### Example of a multiple hierarchy

![Sales 
Region (5) 
Sales 
District (36) 
state (7) 
City (145) 
Customer (10 mill) 
State 
Sales 
Region 
City 
Customer 
Sales 
District ](@attachment/2f5d7569-240c-445f-8796-0579246cda32.png)## Data Warehouses

* Designed to provide the 'data infrastructure' for business intelligence and decision support.
* Lots of different approaches to technology, architecture and design.
### Transaction Processing Systems vs. Data Warehouses

* OLTP (online transaction processing)

    * Data entry, update and deletion
    * High frequency but small volume transactions
    * Usually focussed on one business area
    * Normalised data structures

* DW

    * Data most commonly read
    * Low frequency of transactions, but often large volumes of data
    * Integrates data from systems across multiple business areas
    * Normalised data structures.

## Data Modelling for DW

### The Design of databases using a traditional E-R approach

* Entities and relationships
* Rules of normalisation

    * 3NF is common
    * Protection of integrity of database by avoiding anomalies
    * Every logical thing is represented once only

* Separate consideration logical and physical

    * Normalisation for logical, followed by de-normalisation for the physical model to improve performance.

### Traditional database design

* Large numbers of tables
* Commonly used
* Research shows that they are not easily understood by non IT people
* Optimised for efficient data processing: INSERT, UPDATE, DELETE instead of SELECT
* Complexity hidden form the end user with user interfaces designed around workflows/business processes.
## Summary

* Multi-dimensional data structures area. Common and possibly the best way that business users interact with data in a BI system.
* Multi-dimensional data is an information space that is defined by its facts and dimensions.
* Dimensions consist of members and one or more hierarchies. Dimensions provide the navigation paths with the information space.
* Facts consist of one or more measures. A measure is a numeric value exists at the intersection of a set of dimension members.
* The design of a multi-dimensional data structure should be based on the decision support requirementsNOTon what data is available.
* Multi-dimension domain structure (Thomsen) diagrams are one method of conceptually presenting the design of multi-dimensional data.
