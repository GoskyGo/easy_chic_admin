<template>
  <list-page
    v-if="$can('tax_rule', 'view')"
    ref="listPage"
    list-api="getTaxRules"
    delete-api="deleteTaxRule"
    route-name="tax-rules"
    empty-store-variable="allTaxRules"
    :name="$t('brand.tRule')"
    gate="tax_rule"
    :order-options="orderOptions"
    @delete-bulk="deleteBulk"
    @list="itemList = $event"
  >
    <template v-slot:table="{list}">
        <tr class="lite-bold">
          <th class="w-50x mx-w-50x">
            <input type="checkbox" @change="checkAll">
          </th>
          <th>{{ $t('index.title') }}</th>
          <th>{{ $t('index.price') }}</th>
          <th>{{ $t('index.created') }}</th>
          <th>&nbsp;</th>
        </tr>

        <tr v-for="(item, index) in list" :key="index">
          <td class="w-50x mx-w-50x">
            <input type="checkbox" :value="item.id" v-model="cbList">
          </td>
          <td>
            <nuxt-link
              class="link"
              :to="`/tax-rules/${item.id}`"
            >
              <h5 class="mx-w-300x">{{ item.title }}</h5>
            </nuxt-link>
          </td>
          <td>{{ priceFormat({type: item.type, price: item.price, icon: currencyIcon}) }}</td>
          <td>{{ item.created }}</td>
          <td>
            <button
              v-if="$can('tax_rule', 'edit')"
              @click.prevent="$refs.listPage.editItem(item.id)" class="lite-btn"
            >
              {{ $t('category.edit') }}</button>
            <button
              v-if="$can('tax_rule', 'delete')"
              @click.prevent="$refs.listPage.deleteItem(item.id)"
              class="delete-btn lite-btn"
            >
              {{ $t('category.delete') }}</button>
          </td>
        </tr>
    </template>
  </list-page>
</template>

<script>
  import ListPage from "~/components/partials/ListPage";
  import util from '~/mixin/util'
  import {mapGetters} from 'vuex'
  import bulkDelete from "~/mixin/bulkDelete";

  export default {
    name: "tax-rule",
    middleware: ['common-middleware', 'auth'],
    data(){
      return {
        orderOptions:{
          title: { title: this.$t('index.title') },
          created_at: { title: this.$t('category.date') }
        }
      }
    },
    mixins: [util, bulkDelete],
    components: {
      ListPage
    },
    computed: {
      currencyIcon() {
        return this.setting?.currency_icon || '$'
      },
      ...mapGetters('setting', ['setting'])
    },
    methods:{
    },
    mounted() {
    }
  }
</script>

<style scoped>

</style>
