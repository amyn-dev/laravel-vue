<template>
    <div class="d-flex mb-3">
        <router-link class="ml-auto btn btn-primary btn-sm" :to="{name: 'home'}">
            {{ $t('actions.back') }}
        </router-link>
    </div>
    <div :class="{'loading': loading}">
        <table class="table table-striped table-light">
            <thead class="table-secondary">
            <tr>
                <th scope="col" width="5%">#</th>
                <th scope="col">{{ $t('fields.building') }}</th>
                <th scope="col">{{ $t('fields.note') }}</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="building in buildingsHasNotes" :key="building.id">
                <th scope="row">{{ building.id }}</th>
                <td>{{ building.name }}</td>
                <td class="text-danger">{{ building.notes }}</td>
            </tr>
            </tbody>
        </table>
        <Pagination
            :page="page"
            :viewingAmount="per_page"
            :total="records"
            @change="handlePageUpdate"
        />
    </div>
</template>

<script>
import Pagination from "../../components/Pagination";
import {computed, ref} from "vue";
import {useStore} from "vuex";

export default {
    components: {
        Pagination
    },
    setup() {
        const store = useStore()
        const loading = computed(() => store.state.building.loading);
        const records = computed(() => store.state.building.records);
        const per_page = computed(() => store.state.building.per_page);
        const page = ref(store.state.building.page)

        const buildingsHasNotes = computed(() => store.state.building.buildingsHasNotes);
        if (store.state.loaded_buildingsHasNotes === true) {
            store.dispatch('building/getBuildingsHasNotes', {page: page})
            store.state.loaded_buildingsHasNotes = false
        }

        function handlePageUpdate([p, per_p]) {
            page.value = p
            per_page.value = per_p
            store.dispatch('building/getBuildingsHasNotes', { page: page.value })
        }

        return {
            buildingsHasNotes,
            records,
            per_page,
            page,
            loading,
            handlePageUpdate
        }
    }
}
</script>

<style scoped></style>
