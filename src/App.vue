<template>
  <div class="container">
    <div class="wrapper">
    <ejs-querybuilder :dataSource="dataSource" :ruleChange="onRuleChange" 
      ref="querybuilder" :rule="rule">
      <e-columns>
        <e-column field="EmployeeID" label="Employee ID" type="number"></e-column>
        <e-column field='FirstName' label='First Name' type='string' />
        <e-column field='TitleOfCourtesy' label='Title Of Courtesy' type='string' />
        <e-column field='Title' label='Title' type='string' />
        <e-column field='HireDate' label='Hire Date' type='date' format='dd/MM/yyyy' />
        <e-column field='Country' label='Country' type='string' />
        <e-column field='City' label='City' type='string' />
      </e-columns>
    </ejs-querybuilder>
  </div>
  <div class="wrapper">
    <ejs-grid :dataSource="gridDataSource" ref="grid" :created="onGridCreated">
        <e-columns>
          <e-column field='EmployeeID' width='70' textAlign="Right"></e-column>
          <e-column field='FirstName' width='70' textAlign="Left"></e-column>
          <e-column field='TitleOfCourtesy' width='80' textAlign="Left"></e-column>
          <e-column field='Title' width='100' textAlign="Left"></e-column>
          <e-column field='HireDate' width='80' format="dd/MM/yyyy" textAlign="Right"></e-column>
          <e-column field='Country' width='80' textAlign="Left"></e-column>
          <e-column field='City' width='80' textAlign="Left"></e-column>
        </e-columns>
      </ejs-grid>
  </div>
  </div>
</template>

<script>
import { QueryBuilderComponent, ColumnsDirective, ColumnDirective } from '@syncfusion/ej2-vue-querybuilder';
import { GridComponent } from '@syncfusion/ej2-vue-grids';
import { DataManager, Query } from '@syncfusion/ej2-data';
import { isNullOrUndefined } from '@syncfusion/ej2-base';

let employeeData = [
  {
    'EmployeeID': 1,
    'LastName': 'Davolio',
    'FirstName': 'Nancy',
    'Title': 'Sales Representative',
    'TitleOfCourtesy': 'Ms.',
    'BirthDate': new Date(-664743600000),
    'HireDate': new Date(704692800000),
    'Address': '507 - 20th Ave. E.\r\nApt. 2A',
    'City': 'Seattle',
    'Region': 'WA',
    'PostalCode': '98122',
    'Country': 'USA'
  },
  {
    'EmployeeID': 2,
    'LastName': 'Fuller',
    'FirstName': 'Andrew',
    'Title': 'Vice President',
    'TitleOfCourtesy': 'Dr.',
    'BirthDate': new Date(-563828400000),
    'HireDate': new Date(713764800000),
    'Address': '908 W. Capital Way',
    'City': 'Tacoma',
    'Region': 'WA',
    'PostalCode': '98401',
    'Country': 'USA',
  },
  {
    'EmployeeID': 3,
    'LastName': 'Leverling',
    'FirstName': 'Janet',
    'Title': 'Sales Representative',
    'TitleOfCourtesy': 'Ms.',
    'BirthDate': new Date(-200088000000),
    'HireDate': new Date(702104400000),
    'Address': '722 Moss Bay Blvd.',
    'City': 'Kirkland',
    'Region': 'WA',
    'PostalCode': '98033',
    'Country': 'USA',
  },
  {
    'EmployeeID': 4,
    'LastName': 'Peacock',
    'FirstName': 'Margaret',
    'Title': 'Sales Representative',
    'TitleOfCourtesy': 'Mrs.',
    'BirthDate': new Date(-1018814400000),
    'HireDate': new Date(736401600000),
    'Address': '4110 Old Redmond Rd.',
    'City': 'Redmond',
    'Region': 'WA',
    'PostalCode': '98052',
    'Country': 'USA',
  },
  {
    'EmployeeID': 5,
    'LastName': 'Buchanan',
    'FirstName': 'Steven',
    'Title': 'Sales Manager',
    'TitleOfCourtesy': 'Mr.',
    'BirthDate': new Date(-468010800000),
    'HireDate': new Date(750830400000),
    'Address': '14 Garrett Hill',
    'City': 'London',
    'Region': null,
    'PostalCode':
      'SW1 8JR',
    'Country': 'UK',
  },
  {
    'EmployeeID': 6,
    'LastName': 'Suyama',
    'FirstName': 'Michael',
    'Title': 'Sales Representative',
    'TitleOfCourtesy': 'Mr.',
    'BirthDate': new Date(-205185600000),
    'HireDate': new Date(750830400000),
    'Address': 'Coventry House\r\nMiner Rd.',
    'City': 'London',
    'Region': null,
    'PostalCode': 'EC2 7JR',
    'Country': 'UK',
  },
  {
    'EmployeeID': 7,
    'LastName': 'King',
    'FirstName': 'Robert',
    'Title': 'Sales Representative',
    'TitleOfCourtesy': 'Mr.',
    'BirthDate': new Date(-302731200000),
    'HireDate': new Date(757486800000),
    'Address': 'Edgeham Hollow\r\nWinchester Way',
    'City': 'London',
    'Region': null,
    'PostalCode': 'RG1 9SP',
    'Country': 'UK',
  },
  {
    'EmployeeID': 8,
    'LastName': 'Callahan',
    'FirstName': 'Laura',
    'Title': 'Inside Sales Coordinator',
    'TitleOfCourtesy': 'Ms.',
    'BirthDate': new Date(-377982000000),
    'HireDate': new Date(762843600000),
    'Address': '4726 - 11th Ave. N.E.',
    'City': 'Seattle',
    'Region': 'WA',
    'PostalCode': '98105',
    'Country': 'USA',
  },
  {
    'EmployeeID': 9,
    'LastName': 'Dodsworth',
    'FirstName': 'Anne',
    'Title': 'Sales Representative',
    'TitleOfCourtesy': 'Ms.',
    'BirthDate': new Date(-123966000000),
    'HireDate': new Date(784875600000),
    'Address': '7 Houndstooth Rd.',
    'City': 'London',
    'Region': null,
    'PostalCode': 'WG2 7LT',
    'Country': 'UK',
  },
  {
    'EmployeeID': 10,
    'LastName': 'Smith',
    'FirstName': 'John',
    'Title': 'Marketing Manager',
    'TitleOfCourtesy': 'Mr.',
    'BirthDate': new Date(-1104556800000),
    'HireDate': new Date(821328000000),
    'Address': '123 Main St',
    'City': 'New York',
    'Region': 'NY',
    'PostalCode': '10001',
    'Country': 'USA',
  },
  {
    'EmployeeID': 11,
    'LastName': 'Johnson',
    'FirstName': 'Emily',
    'Title': 'HR Specialist',
    'TitleOfCourtesy': 'Ms.',
    'BirthDate': new Date(-956985600000),
    'HireDate': new Date(916604400000),
    'Address': '456 Elm St',
    'City': 'Chicago',
    'Region': 'IL',
    'PostalCode': '60601',
    'Country': 'USA',
  },
  {
    'EmployeeID': 12,
    'LastName': 'Taylor',
    'FirstName': 'Rachel',
    'Title': 'Marketing Specialist',
    'TitleOfCourtesy': 'Ms.',
    'BirthDate': new Date(-853305600000),
    'HireDate': new Date(954506400000),
    'Address': '789 Pine St',
    'City': 'San Francisco',
    'Region': 'CA',
    'PostalCode': '94108',
    'Country': 'USA',
  },
  {
    'EmployeeID': 13,
    'LastName': 'Adams',
    'FirstName': 'Michael',
    'Title': 'Financial Analyst',
    'TitleOfCourtesy': 'Mr.',
    'BirthDate': new Date(-789820800000),
    'HireDate': new Date(1025558400000),
    'Address': '1010 Oak Ave',
    'City': 'New York',
    'Region': 'NY',
    'PostalCode': '10016',
    'Country': 'USA',
  }
];

export default {
  components: {
    "ejs-querybuilder": QueryBuilderComponent,
    "e-columns": ColumnsDirective,
    "e-column": ColumnDirective,
    "ejs-grid": GridComponent
  },
  data(){
    return {
      dataSource: employeeData,
      gridDataSource: employeeData,
      rule:{
        'condition': 'and',
        'rules':[{
          'field': 'Title',
          'operator': 'startswith',
          'value': 'Sales'
        },
        {
          'field': 'Country',
          'operator': 'startswith',
          'value': 'UK'
        }]
      }
    };
  },
  methods:{
    onRuleChange(args){
      const columnsToSelect = ["EmployeeID", "FirstName", "TitleOfCourtesy", "Title", "HireDate", "Country", "City"];
      const predicate = this.$refs.querybuilder.ej2Instances.getPredicate(args.rule);
      let query = new Query().select(columnsToSelect);

      if(!isNullOrUndefined(predicate)){
        query = query.where(predicate);
      }
      new DataManager(this.dataSource)
        .executeQuery(query)
        .then((e) => {
          this.gridDataSource = e.result;
          this.$refs.grid.ej2Instances.refresh();
        });
    },
    onGridCreated(){
      this.onRuleChange({
        rule: this.$refs.querybuilder.ej2Instances.getValidRules(
          this.$refs.querybuilder.ej2Instances.rule
        )
      });
    }
  }
}
</script>
<style>
@import "../node_modules/@syncfusion/ej2-base/styles/material.css";
@import "../node_modules/@syncfusion/ej2-buttons/styles/material.css";
@import "../node_modules/@syncfusion/ej2-splitbuttons/styles/material.css";
@import "../node_modules/@syncfusion/ej2-dropdowns/styles/material.css";
@import "../node_modules/@syncfusion/ej2-inputs/styles/material.css";
@import "../node_modules/@syncfusion/ej2-lists/styles/material.css";
@import "../node_modules/@syncfusion/ej2-popups/styles/material.css";
@import "../node_modules/@syncfusion/ej2-calendars/styles/material.css";
@import "../node_modules/@syncfusion/ej2-vue-querybuilder/styles/material.css";

@import "../node_modules/@syncfusion/ej2-navigations/styles/material.css";
@import "../node_modules/@syncfusion/ej2-vue-grids/styles/material.css";

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 60%;
  margin: 0 auto;
  position: absolute;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
}

.wrapper {
  width: 100%;
  padding: 10px;
}
</style>
