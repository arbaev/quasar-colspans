<script setup>
import { onMounted, ref } from "vue";
import TableHeader from "components/TableHeader.vue";

const rowsData = [
  // Data structure for the table from API
  // Nested objects form the table cells of the colspan
  // To draw the table, we convert this object to flat one
  // { month: { plan: 3, close: 5 }} => { monthPlan: 3, monthClose: 5 }
  {
    id: 1,
    userData: {
      name: "Alex",
      surname: "Ronin",
      age: 35,
    },
    task: "task 1",
    month: {
      plan: 10,
      close: 20,
    },
  },
  {
    id: 2,
    userData: {
      name: "Karate",
      surname: "Kid",
      age: 12,
    },
    task: "task 2",
    month: {
      plan: 30,
      close: 55,
    },
  },
  {
    id: 3,
    userData: {
      name: "Kate",
      surname: "Flush",
      age: 22,
    },
    task: "task 3",
    month: {
      plan: 15,
      close: 18,
    },
  },
];

const headerCellsList = [
  // Array of table header cells for the first row (with colspans and rowspans)
  // and for the second row. Order matters.
  // The name of the cells for the second row is formed by adding the parent cell name to its name
  // The table header will look like this:
  // |----|----------------------------------------------|------|------------------------|
  // |    |                   userData                   |      |         month          |
  // | id |----------------------------------------------| task |------------------------|
  // |    | userDataName | userDataSurname | userDataAge |      | monthPlan | monthClose |
  // |----|----------------------------------------------|------|------------------------|
  [
    // first row
    "id",
    "userData",
    "task",
    "month",
  ],
  [
    // second row
    "userDataName",
    "userDataSurname",
    "userDataAge",

    "monthPlan",
    "monthClose",
  ],
];

const colspans = [
  // Description of colspans and css classes for borders etc
  {
    name: "userData",
    label: "User's data",
    thClasses: "border-bold-right border-bold-top",
  },
  {
    name: "month",
    label: "Month's tasks",
    thClasses: "border-bold-right border-bold-top",
  },
];

const columns = [
  // Descriptions of columns (all except colspans)
  {
    name: "id",
    field: "id",
    label: "Id",
    rowspan: 2,
    required: true,
    sortable: true,
    align: "right",
    thClasses: "border-bold-left border-bold-right", // classes for header
    classes: "border-bold-left border-bold-right", // classes for column
  },
  {
    name: "userDataName",
    field: "userDataName",
    label: "Name",
    group: "userData", // group that's the colspan belong to
    required: true,
    sortable: true,
    align: "left",
  },
  {
    name: "userDataSurname",
    field: "userDataSurname",
    label: "Surname",
    group: "userData", // group that's the colspan belong to
    required: true,
    sortable: true,
    align: "left",
  },
  {
    name: "userDataAge",
    field: "userDataAge",
    label: "Age",
    group: "userData", // group that's the colspan belong to
    required: true,
    sortable: true,
    thClasses: "border-bold-right",
    classes: "border-bold-right",
    align: "right",
  },
  {
    name: "task",
    field: "task",
    label: "Task",
    rowspan: 2,
    required: true,
    sortable: true,
    thClasses: "border-bold-top border-bold-right",
    classes: "border-bold-right",
    align: "left",
  },
  {
    name: "monthPlan",
    field: "monthPlan",
    label: "Month Plan",
    group: "month",
    format: (val) => `${val}%`,
    required: true,
    sortable: true,
    align: "right",
  },
  {
    name: "monthClose",
    field: "monthClose",
    label: "Month Close",
    group: "month",
    format: (val) => val.toFixed(2),
    required: true,
    sortable: true,
    thClasses: "border-bold-right",
    classes: "border-bold-right",
    align: "right",
  },
];

const capitalize = (str) => {
  // capitalizes the first letter of the string
  return str && str[0].toUpperCase() + str.slice(1);
};
const flattenObject = (ob) => {
  // flattens an object with nested properties
  // { month: { plan: 3, close: 5 }} => { monthPlan: 3, monthClose: 5 }
  const result = {};

  for (const i in ob) {
    if (!ob.hasOwnProperty(i)) continue;

    if (typeof ob[i] == "object" && ob[i] !== null) {
      const flatObject = flattenObject(ob[i]);
      for (const x in flatObject) {
        if (!flatObject.hasOwnProperty(x)) continue;

        result[i + capitalize(x)] = flatObject[x];
      }
    } else {
      result[i] = ob[i];
    }
  }
  return result;
};

const rows = ref([]);
const fetchProjectsReport = onMounted(() => {
  // data loading stub
  rows.value = rowsData.map((r) => flattenObject(r));
});
</script>

<template>
  <q-page class="flex">
    <section class="table-wrap">
      <q-table
        id="maintable"
        :columns="columns"
        :rows="rows"
        row-key="id"
        separator="none"
        class="q-pa-lg"
        flat
        square
        virtual-scroll
        wrap-cells
        binary-state-sort
        hide-pagination
      >
        <template v-slot:header="props">
          <table-header
            :props="props"
            :colspanData="colspans"
            :headerCellsList="headerCellsList"
          />
        </template>
      </q-table>
    </section>
  </q-page>
</template>

<style lang="scss">
// HEADER TEXT start
.table-header th {
  font-weight: bolder;
  font-size: 0.9em;
}
.table-header:first-child {
  font-size: 1.2em;
}
// HEADER TEXT end

// BORDERS start
.sticky-td:first-child {
  background-color: #fff;
  border-left: solid black 2px !important;
  border-right: solid black 2px !important;
}
thead tr:first-child th:first-child {
  border-top: solid black 2px !important;
  border-left: solid black 2px !important;
  border-right: solid black 2px !important;
}
.q-table tbody td,
.td-cell {
  border-right: solid 1px rgba(0, 0, 0, 0.5);
  border-bottom: solid 1px rgba(0, 0, 0, 0.5);
}
.table-header th {
  border-bottom: solid black 2px !important;
}
.th-cell {
  border-right: solid 1px rgba(0, 0, 0, 0.5) !important;
}
.border-bold-left {
  border-left: solid black 2px !important;
}
.border-bold-right {
  border-right: solid black 2px !important;
}
.border-bold-top {
  border-top: solid black 2px !important;
}
.border-bold-bottom {
  border-bottom: solid black 2px !important;
}
// BORDERS end
</style>
