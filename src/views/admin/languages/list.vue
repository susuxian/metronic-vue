<!-- Done Reviewing: 30/01/2023 -->
<template>
  <toolbar title="Languages Management" />
  <add-language-modal
    ref="addLanguageModal"
    :id-current="idCurrent"
    :language-current="languageCurrent" />
  <div id="kt_app_content" class="app-content flex-column-fluid">
    <div id="kt_app_content_container" class="app-container container-xxl">
      <div class="card">
        <template v-if="data.length === 0">
          <div class="card-body p-0">
            <div class="card-px text-center py-20 my-10">
              <h2 class="fs-2x fw-bold mb-10">Welcome!</h2>
              <p class="text-gray-400 fs-5 fw-semibold mb-13">
                <span>
                  There are no languages added yet.
                  <br />
                  Kickstart your dashboard by adding a your first language
                </span>
              </p>
              <button type="button" class="btn btn-primary er fs-6 px-8 py-4" @click="showAddModal">
                {{ t("main.add-button") }} {{ t("main.language") }}
              </button>
            </div>
            <div class="text-center px-5">
              <img
                src="@/assets/media/illustrations/01.png"
                alt="Add Language Illustration"
                class="mw-100 mh-300px" />
            </div>
          </div>
        </template>
        <template v-else>
          <div class="card-header border-0 pt-6">
            <div class="card-title">
              <div class="d-flex align-items-center position-relative my-1">
                <span class="svg-icon svg-icon-1 position-absolute ms-6">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    width="24"
                    height="24"
                    fill="none">
                    <rect
                      x="17"
                      y="15"
                      rx="1"
                      width="8"
                      height="2"
                      transform="rotate(45 17 15)"
                      fill="currentColor"
                      opacity="0.5" />
                    <path
                      d="M11 19C6.55556 19 3 15.4444 3 11C3 6.55556 6.55556 3 11 3C15.4444 3 19 6.55556 19 11C19 15.4444 15.4444 19 11 19ZM11 5C7.53333 5 5 7.53333 5 11C5 14.4667 7.53333 17 11 17C14.4667 17 17 14.4667 17 11C17 7.53333 14.4667 5 11 5Z"
                      fill="currentColor" />
                  </svg>
                </span>
                <label for="search-languages" class="sr-only">Search Languages</label>
                <input
                  id="search-languages"
                  type="text"
                  name="search-languages"
                  placeholder="Search Languages"
                  data-kt-language-table-filter="search"
                  class="form-control form-control-solid w-250px ps-14" />
              </div>
            </div>
            <div class="card-toolbar">
              <div data-kt-language-table-toolbar="base" class="d-flex justify-content-end">
                <button type="button" class="btn btn-primary" @click="showAddModal">
                  <span class="svg-icon svg-icon-2">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      width="24"
                      height="24"
                      fill="none">
                      <rect
                        x="11"
                        y="20"
                        rx="1"
                        width="16"
                        height="2"
                        transform="rotate(-90 11 20)"
                        fill="currentColor"
                        opacity="0.5" />
                      <rect x="4" y="11" rx="1" width="16" height="2" fill="currentColor" />
                    </svg>
                  </span>
                  {{ t("main.add-button") }} {{ t("main.language") }}
                </button>
              </div>
            </div>
          </div>
          <div class="card-body py-4">
            <data-table
              :data="data"
              :header="header"
              :checkbox-enabled="true"
              checkbox-label="id"
              :items-total="itemsTotal"
              :items-per-page-dropdown-enabled="true"
              @on-sort="onSort"
              @on-items-select="onItemsSelect">
              <template #name="{row: language}">
                {{ language.name.charAt(0).toUpperCase() + language.name.slice(1) }}
              </template>
              <template #shortname="{row: language}">
                {{ language.shortname.toUpperCase() }}
              </template>
              <template #direction="{row: language}">
                {{ language.direction.toUpperCase() }}
              </template>
              <template #dirword="{row: language}">
                {{ language.dirword.charAt(0).toUpperCase() + language.dirword.slice(1) }}
              </template>
              <template #icon="{row: language}">
                <img
                  :src="`../../../src/assets/media/flags/${language.icon}`"
                  :alt="`${language.icon}`"
                  class="w-20px h-20px rounded-1 ms-2" />
              </template>
              <template #actions="{row: language}">
                <a
                  href="#"
                  data-kt-menu-trigger="click"
                  data-kt-menu-placement="bottom-end"
                  data-kt-menu-flip="top-end"
                  class="btn btn-sm btn-light btn-active-light-primary">
                  {{ t("main.actions-button") }}
                  <span class="svg-icon svg-icon-5 m-0">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      width="24"
                      height="24"
                      fill="none">
                      <path
                        d="M11.4343 12.7344L7.25 8.55005C6.83579 8.13583 6.16421 8.13584 5.75 8.55005C5.33579 8.96426 5.33579 9.63583 5.75 10.05L11.2929 15.5929C11.6834 15.9835 12.3166 15.9835 12.7071 15.5929L18.25 10.05C18.6642 9.63584 18.6642 8.96426 18.25 8.55005C17.8358 8.13584 17.1642 8.13584 16.75 8.55005L12.5657 12.7344C12.2533 13.0468 11.7467 13.0468 11.4343 12.7344Z"
                        fill="currentColor"></path>
                    </svg>
                  </span>
                </a>
                <div
                  data-kt-menu="true"
                  class="menu menu-sub menu-sub-dropdown menu-column menu-rounded menu-gray-600 menu-state-bg-light-primary fw-semibold fs-7 w-125px py-4">
                  <div class="menu-item px-3">
                    <a href="#" class="menu-link px-3" @click="showUpdateModal(language.id)">
                      {{ t("main.update-button") }}
                    </a>
                  </div>
                  <div class="menu-item px-3">
                    <a href="#" class="menu-link px-3" @click="deleteLanguage(language.id)">
                      {{ t("main.delete-button") }}
                    </a>
                  </div>
                </div>
              </template>
            </data-table>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import Toolbar from "@/components/admin/dashboard/toolbar.vue"
import DataTable from "@/components/admin/data-table/index.vue"
import AddLanguageModal from "@/components/admin/modals/forms/add-language-modal.vue"
import axiosClient from "@/plugins/axios"
import arraySort from "array-sort"
import {defineComponent, onBeforeMount, onMounted, provide, ref} from "vue"
import {useI18n} from "vue-i18n"
import {showModal} from "../../../core/helpers/dom"

export default defineComponent({
  name: "languages-list",
  components: {Toolbar, AddLanguageModal, DataTable},
  setup() {
    const {t} = useI18n({useScope: "global"})
    const header = ref([
      {columnName: "Language Name", columnLabel: "name", sortEnabled: true, columnWidth: 230},
      {columnName: "Short Name", columnLabel: "shortname", sortEnabled: true, columnWidth: 175},
      {columnName: "Direction", columnLabel: "direction", sortEnabled: true, columnWidth: 175},
      {columnName: "Direction Word", columnLabel: "dirword", sortEnabled: true, columnWidth: 175},
      {columnName: "Icon", columnLabel: "icon", sortEnabled: false, columnWidth: 175},
      {columnName: "Actions", columnLabel: "actions", sortEnabled: false, columnWidth: 175}
    ])

    const data = ref([])
    const itemsTotal = ref(0)
    const initLanguages = ref([])
    const idsSelected = ref([])
    const addLanguageModal = ref(null)
    const idCurrent = ref(null)
    const languageCurrent = ref({
      name: null,
      shortname: null,
      direction: null,
      dirword: null,
      icon: null
    })

    const getDataTableBodyRows = function getDataTableBodyRows(queryString = "") {
      axiosClient.get(`/languages${queryString}`).then((response) => {
        data.value = response.data.result.data
        itemsTotal.value = response.data.result.total
      })
    }

    const deleteLanguage = function deleteLanguage(id) {
      axiosClient.delete(`/languages/delete/${id}`).then(() => {
        getDataTableBodyRows()
      })
    }

    const deleteFewLanguages = function deleteFewLanguages() {
      idsSelected.value.forEach((item) => {
        deleteLanguage(item)
      })

      idsSelected.value.length = 0
    }

    const onSort = function onSort(sort) {
      const reverse = sort.order === "ASC".toLowerCase()
      if (sort.label) arraySort(data.value, sort.label, {reverse})
    }

    const onItemsSelect = function onItemsSelect(itemsSelected) {
      if (itemsSelected.length === 0) idsSelected.value = []
      else idsSelected.value = [...idsSelected.value, ...itemsSelected]
    }

    const showAddModal = function showAddModal() {
      idCurrent.value = null
      languageCurrent.value = {
        name: null,
        shortname: null,
        direction: null,
        dirword: null,
        icon: null
      }

      showModal(addLanguageModal.value.addLanguageModal.modal)
    }

    const showUpdateModal = function showUpdateModal(id) {
      idCurrent.value = id
      axiosClient.get(`/languages/show/${id}`).then((response) => {
        languageCurrent.value = response.data.result
        showModal(addLanguageModal.value.addLanguageModal.modal)
      })
    }

    provide("getDataTableBodyRows", getDataTableBodyRows)
    onBeforeMount(() => {
      data.value = []
      getDataTableBodyRows()
    })

    onMounted(() => {
      initLanguages.value.splice(0, data.value.length, ...data.value)
    })

    return {
      t,
      header,
      data,
      itemsTotal,
      idsSelected,
      addLanguageModal,
      idCurrent,
      languageCurrent,
      onSort,
      onItemsSelect,
      deleteLanguage,
      deleteFewLanguages,
      showAddModal,
      showUpdateModal
    }
  }
})
</script>
