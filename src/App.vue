<template>
  <div id="app">
    <ejs-grid ref="grid" :dataSource="data" height="315px" :created="created">
      <e-columns>
        <e-column
          field="OrderID"
          headerText="Order ID"
          textAlign="Right"
          :isPrimaryKey="true"
          width="120"
        ></e-column>
        <e-column
          field="OrderDate"
          headerText="Order Date"
          width="130"
          textAlign="Right"
          format="yMd"
        ></e-column>
        <e-column
          field="ShipCountry"
          headerText="Ship Country"
          width="120"
        ></e-column>
        <e-column
          field="Freight"
          headerText="Receipt Amount"
          width="120"
          :template="cTemplate"
        ></e-column>
      </e-columns>
    </ejs-grid>
  </div>
</template>
<script>
import Vue from 'vue';
import {
  GridPlugin,
  parentsUntil,
  GridComponent,
  ColumnDirective,
  ColumnsDirective,
} from '@syncfusion/ej2-vue-grids';

import data from './datasource.js';

export default {
  components: {
    'ejs-grid': GridComponent,
    'e-column': ColumnDirective,
    'e-columns': ColumnsDirective,
  },
  data() {
    return {
      data: [
      {
        OrderID: 10248,
        CustomerID: 'VINET',
        OrderDate: '1996-07-04T00:00:00.000Z',
        ShippedDate: '1996-07-16T00:00:00.000Z',
        Freight: 32.38,
        ShipName: 'Vins et alcools Chevalier',
        ShipAddress: "59 rue de l'Abbaye",
        ShipCity: 'Reims',
        ShipRegion: null,
        ShipCountry: 'France',
        Verified: true,
      },
      {
        OrderID: 10249,
        CustomerID: 'TOMSP',
        OrderDate: '1996-07-05T00:00:00.000Z',
        ShippedDate: '1996-07-10T00:00:00.000Z',
        Freight: 11.61,
        ShipName: 'Toms Spezialitäten',
        ShipAddress: 'Luisenstr. 48',
        ShipCity: 'Münster',
        ShipRegion: null,
        ShipCountry: 'Germany',
      }],
      cTemplate: function () {
        return {
          template: Vue.component('columnTemplate', {
            template: `<div class="image">
                 <input :id='id' :value='value' class='custemp' type='text' style='width: 100%'/>
            </div>`,
            data: function () {
              return {
                data: {},
              };
            },
            computed: {
              id: function () {
                return this.data.OrderID;
              },
              value: function () {
                return this.data.Freight;
              },
            },
          }),
        };
      },
    };
  },
  methods: {
    created(args) {
      this.$refs.grid.ej2Instances.element.addEventListener(
        'keyup',
        function (e) {
          // Bind the keyup event for the grid.
          if (e.target.classList.contains('custemp')) {
            // Based on this condition, you can find whether the target is an input element or not.
            var row = parentsUntil(e.target, 'e-row');
            var rowIndex = row.rowIndex; // Get the row index.
            var uid = row.getAttribute('data-uid');
            var grid =
              document.getElementsByClassName('e-grid')[0].ej2_instances[0];
            var rowData = grid.getRowObjectFromUID(uid).data; // Get the row data.
            rowData.Freight = e.target.value; // Update the new value for the corresponding column.
            grid.updateRow(rowIndex, rowData); // Update the modified value in the row data.
          }
        }
      );
    },
  },
};
</script>
<style>
@import '@syncfusion/ej2-vue-grids/styles/material.css';
</style>
