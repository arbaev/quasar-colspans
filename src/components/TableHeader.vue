<script>
// Component for building and displaying table headers
// 1. Props data is:
//    a) list of columns inside props,
//    b) list of colspan cells "colspanData"
//    c) order of columns in the list "headerCellsList"
// 2. Calculate colspan attributes based on columns (countColspans),
//    to do this, combine columns and colspanData into one object columnsData
// 3. Generate lists of columns for the first and second rows of the table header,
//    based on columnsData and headerCellsList (rowFirst and rowSecond)

export default {
  name: "TableHeader",
  props: ["props", "colspanData", "headerCellsList"],
  data() {
    return {
      columnsData: [],
      rowFirst: [],
      rowSecond: [],
    };
  },
  watch: {
    props() {
      this.setTableHeader();
    },
  },
  created() {
    this.setTableHeader();
  },
  methods: {
    setTableHeader() {
      // header lines calculation
      this.columnsData = [...this.colspanData, ...this.props.cols];
      this.setColspans();
      this.rowFirst = this.setRow(this.headerCellsList[0]);
      this.rowSecond = this.setRow(this.headerCellsList[1]);
    },
    setColspans() {
      // sets the colspan attribute with the calculated value in columnsData
      const colspanObj = this.countColspans();
      this.deleteColspans(this.columnsData);
      for (let i in colspanObj) {
        const cell = this.columnsData.find((c) => i === c.name);
        cell.colspan = colspanObj[i];
      }
    },
    deleteColspans(columns) {
      columns.forEach((cell) => delete cell.colspan);
    },
    countColspans() {
      // based on columns, creates an object with the number of columns and their name
      // { "monthlyplan": 4, "thisweek": 4, "urgenttask": 3 }
      return this.props.cols.reduce((acc, col) => {
        if ("group" in col) {
          let quantity = acc[col.group];
          acc[col.group] = typeof quantity === "undefined" ? 1 : quantity + 1;
        }
        return acc;
      }, {});
    },
    setRow(list) {
      // from the array of columns columnsData generate an array of columns
      // in accordance with the list of columns passed in the parameters
      return list.reduce((acc, cellName) => {
        const cell = this.columnsData.find((c) => c.name === cellName);
        cell === undefined ? null : acc.push(cell);
        return acc;
      }, []);
    },
  },
};
</script>

<template>
  <q-tr class="table-header table-header__firstrow">
    <q-th
      v-for="th in rowFirst"
      :key="th.name"
      :props="typeof th.colspan === 'undefined' ? props : undefined"
      :colspan="th.colspan"
      :rowspan="th.rowspan"
      :class="th.thClasses"
      class="th-cell"
    >
      <span v-html="th.label"></span>
    </q-th>
  </q-tr>

  <q-tr class="table-header">
    <q-th
      v-for="th in rowSecond"
      :key="th.name"
      :props="props"
      :class="th.classes"
      class="th-cell"
    >
      <span v-html="th.label"></span>
    </q-th>
  </q-tr>
</template>
