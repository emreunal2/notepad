# Split 08/11/2021-15/11/2021

* **In mp-v4 dropdown menus corrupted, first of all we need to fix it to start work**

## 1. Dashboard Changes

* Dashboard HTML codes are in templates/companies.html
* Be careful about almost every code had a queries.py connection so you should check AJAX codes before changing anything

- [X] Planing
- [ ] Analysis
- [X] Design
- [ ] Implementation
- [ ] Testing & Integration
- [ ] Maintenance

### A. Date format change to British format

* In the dashboard page, date format should be as British but since date format picker is 3rd party API, we should look into documantation of date range picker.
- [ ] **IMPORTANT: If we send queries as british format queries can be corrupt because mindate-maxdate**
- [ ] Read docs of Date Range Picker and change it to British version, **IF** there is no module for British format change date range picker
- [ ] Talk about this with Cenk Bey

### B. Dropdown Menu changes

- [ ] In Dashboard, dropdown menues are oldest year to current year, we should change this to current year to oldest year.
- [ ] But since we sending queries from there, we should also change AJAX code's to new changes.

### C. Remove Allocations

- [X] Remove Allocations from bottom of Dashboard.
- [X] Since Allocations removed make Daily Availability Report to full size.
- [X] For better optimization remove Allocation Queries as well from dashboard.

## 2. Cancelation Page Changes

* Cancelation Page HTML files are in templates/pages/finance/cancelations.html
* This page coded by Emre so if cant find anything or some problems ask it directly to Emre
 
- [X] Planing
- [ ] Analysis
- [ ] Design
- [ ] Implementation
- [ ] Testing & Integration
- [ ] Maintenance

### A. Total Amount Chart

- [ ] Create a new chart like ratio chart but in this one dont use ratio and only **losed money**.
- [ ] But there is not a data as **losed money** in database so ask it to Cenk Bey.

### B. Cancelations total amount will be put
- [ ] There is no clear info about this task, ask it to Cenk Bey
