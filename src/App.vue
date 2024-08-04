<script setup>
import { ref } from 'vue';

const drinks = ref([
  { name: '珍珠奶茶', description: '香濃奶茶搭配QQ珍珠', price: 50, stock: 20, is_edit: false },
  { name: '冬瓜檸檬', description: '清新冬瓜配上新鮮檸檬', price: 45, stock: 18, is_edit: false },
  { name: '翡翠檸檬', description: '綠茶與檸檬的完美結合', price: 55, stock: 34, is_edit: false },
  { name: '四季春茶', description: '香醇四季春茶，回甘無比', price: 45, stock: 10, is_edit: false },
  { name: '阿薩姆奶茶', description: '阿薩姆紅茶搭配香醇鮮奶', price: 50, stock: 25, is_edit: false },
  { name: '檸檬冰茶', description: '檸檬與冰茶的清新組合', price: 45, stock: 20, is_edit: false },
  { name: '芒果綠茶', description: '芒果與綠茶的獨特風味', price: 55, stock: 18, is_edit: false },
  { name: '抹茶拿鐵', description: '抹茶與鮮奶的絕配', price: 60, stock: 20, is_edit: false },
])

const tempDrinks = ref({});


function editItem(index) {
  tempDrinks.value = { ...drinks.value[index] };
  drinks.value[index].is_edit = true;
}

function confirmEditItem(index) {
  tempDrinks.value.price =
    checkPrice(drinks.value[index].price, tempDrinks.value.price);
  drinks.value[index] = { ...tempDrinks.value };

  drinks.value[index].is_edit = false;
  tempDrinks.value = {};
}

function addStock(index) {
  drinks.value[index].stock++;
  checkStock(index);
}

function minusStock(index) {
  drinks.value[index].stock--;
  checkStock(index);
}

function checkStock(index) {
  if (drinks.value[index].stock < 0) {
    drinks.value[index].stock = 0;
  }
}

function checkPrice(origin_price, price) {
  console.log(origin_price, price);
  if (price == '' || isNaN(price)) {
    return origin_price;
  }
  return price < 0 ? 0 : price;
}
</script>
j
<template>
  <table>
    <thead>
      <tr>
        <th scope="col">品項</th>
        <th scope="col">描述</th>
        <th scope="col">價格</th>
        <th scope="col">庫存</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in drinks" :key="index">
        <td>
          <span :class="{ 'd-none': item.is_edit }">{{ item.name }}</span>
          <input type="text" v-model="tempDrinks.name" :class="{ 'd-none': !item.is_edit }">
        </td>
        <td>
          <small :class="{ 'd-none': item.is_edit }">{{ item.description }}</small>
          <input type="text" v-model="tempDrinks.description" :class="{ 'd-none': !item.is_edit }">
        </td>
        <td>
          <span :class="{ 'd-none': item.is_edit }">{{ item.price }}</span>
          <input type="number" v-model.number="tempDrinks.price" :class="{ 'd-none': !item.is_edit }" min="0">
        </td>
        <td>
          <button type="button" @click="minusStock(index)">-</button>
          <span>{{ item.stock }}</span>
          <button type="button" @click="addStock(index)">+</button>
        </td>
        <td>
          <button type="button" @click="editItem(index)" :class="{ 'd-none': item.is_edit }">編輯</button>
          <button type="button" @click="confirmEditItem(index)" :class="{ 'd-none': !item.is_edit }">確定</button>
          <button type="button" @click="item.is_edit = false" :class="{ 'd-none': !item.is_edit }">取消</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<style>
.d-none {
  display: none;
}
</style>