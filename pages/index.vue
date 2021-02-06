<template>
  <p v-if="$fetchState.error">An error occurred :(</p>
  <div v-else>
    <section>
      <b-table
        :data="data"
        :paginated="isPaginated"
        :per-page="perPage"
        :current-page.sync="currentPage"
        :pagination-simple="isPaginationSimple"
        :pagination-position="paginationPosition"
        :default-sort-direction="defaultSortDirection"
        :pagination-rounded="isPaginationRounded"
        :sort-icon="sortIcon"
        :sort-icon-size="sortIconSize"
        :loading="$fetchState.pending"
        default-sort="date"
        aria-next-label="Next page"
        aria-previous-label="Previous page"
        aria-page-label="Page"
        aria-current-label="Current page"
      >
        <b-table-column
          field="date"
          label="Date"
          sortable
          centered
          v-slot="props"
        >
          <span>
            {{ new Date(props.row.date).toLocaleDateString('lt') }}
            {{ new Date(props.row.date).toLocaleTimeString('de-AT') }}
          </span>
        </b-table-column>

        <b-table-column field="action" label="Action" sortable v-slot="props">
          <b-tooltip v-if="props.row.action == '2'" label="Zugesperrt">
            <b-icon icon="lock"> </b-icon>
          </b-tooltip>
          <b-tooltip v-else-if="props.row.action == '3'" label="Aufgesperrt">
            <b-icon icon="lock-open-variant-outline"> </b-icon>
          </b-tooltip>
          <b-tooltip v-else-if="props.row.action == '240'" label="Tür geöffnet">
            <b-icon icon="door-open"> </b-icon>
          </b-tooltip>
          <b-tooltip
            v-else-if="props.row.action == '241'"
            label="Tür geschlossen"
          >
            <b-icon icon="door-closed"> </b-icon>
          </b-tooltip>
          <span v-else>{{ props.row.action }}</span>
        </b-table-column>

        <b-table-column field="name" label="Name" sortable v-slot="props">
          {{ props.row.name }}
        </b-table-column>
      </b-table>
    </section>

    <b-field grouped group-multiline>
      <b-select v-model="perPage" :disabled="!isPaginated">
        <option value="5">5 per page</option>
        <option value="10">10 per page</option>
        <option value="15">15 per page</option>
        <option value="20">20 per page</option>
      </b-select>
      <div class="control is-flex">
        <b-switch v-model="isDoorStateShown" @input="filterDoorStates"
          >Tür Status anzeigen</b-switch
        >
      </div>
    </b-field>
  </div>
</template>

<script>
export default {
  methods: {
    filterDoorStates() {
      if (!this.isDoorStateShown) {
        this.data = this.data.filter((elem) => [2, 3].includes(elem.action))
      } else {
        this.$fetch()
      }
    },
  },
  data() {
    return {
      data: [],
      isPaginated: true,
      isPaginationSimple: false,
      isPaginationRounded: false,
      paginationPosition: 'bottom',
      defaultSortDirection: 'desc',
      sortIcon: 'arrow-up',
      sortIconSize: 'is-small',
      currentPage: 1,
      perPage: 10,
      isDoorStateShown: false,
    }
  },
  activated() {
    if (!this.isDoorStateShown) {
      this.data = this.data.filter((elem) => [2, 3].includes(elem.action))
    }
  },
  async fetch() {
    const response = await fetch('https://api.nuki.io/smartlock/log?limit=40', {
      headers: {
        Authorization:
          'Bearer e9bff49447c33a52746e84751773384b546fcd23d96c519fdf0beef8b1c4af1ec8c21bc185b21f4c',
      },
    })
    this.data = await response.json()
  },
}
</script>
