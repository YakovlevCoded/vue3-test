<template>
  <query-builder :cubejs-api="cubejsApi" :query="query">
    <template #builder="{ availableMeasures, setMeasures }">
      <select v-model="value" @change="change(setMeasures)">
        <option v-for="(measure, key) in availableMeasures" :key="key"
        :value="measure"
        >{{measure}}</option>
      </select>
    </template>

    <template #default="{ resultSet }">
      {{resultSet}}
      <!-- <chart-renderer v-if="resultSet" :result-set="resultSet" /> -->
    </template>

    <template #empty>Loading...</template>
  </query-builder>
</template>

<script>
import cubejs from '@cubejs-client/core';
import { QueryBuilder } from '@cubejs-client/vue3';

const API_URL = 'https://ecom.cubecloudapp.dev/cubejs-api/v1';
const CUBEJS_TOKEN =
  'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE1OTQ2NjY4OTR9.0fdi5cuDZ2t3OSrPOMoc3B1_pwhnWj4ZmM3FHEX7Aus';

const cubejsApi = cubejs(CUBEJS_TOKEN, { apiUrl: API_URL });

export default {
  name: 'QueryBuilderExample',
  components: {
    QueryBuilder,
  },
  data() {
    const query = {
      measures: [],
      timeDimensions: [
        {
          dimension: 'LineItems.createdAt',
          granularity: 'month',
        },
      ],
    };

    return {
      cubejsApi,
      selected: undefined,
      query,
      value: null
    };
  },
  methods: {
    customLabel(a) {
      return a.title;
    },
    set(setMeasures, value) {
      setMeasures(value.map((e) => e.name));
    },
    change(setMeasures) {
      setMeasures([this.value.name]);
    }
  },
};
</script>