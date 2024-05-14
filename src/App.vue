<script setup>
import { ref } from 'vue';

const header = ref('Shopping List App');
let num = ref(0);
let newItem = ref("");
let editing = ref(false);
let newItemHighPriority = ref(false);
let items = ref([
                {item: "ball", id: num.value++, purchased: true, highPriority: false}, 
                {item: "racket", id: num.value++, purchased: false, highPriority: true}, 
                {item: "net", id: num.value++, purchased: false, highPriority: false}
                 ]);
const saveItem = () =>{
  items.value.push({item: newItem.value, 
                    id: num.value++, 
                    purchased: false, 
                    highPriority: newItemHighPriority.value});
  newItem.value = "";
  newItemHighPriority.value = false;
};

const enableEdit = (e) => {
  editing.value = e;
  newItem.value = "";
  newItemHighPriority.value = false;
};

const togglePurchased = (key) => {
  items.value[key].purchased = !(items.value[key].purchased);
};


</script>

<template>
  <h1>{{ header?.toLocaleUpperCase()}}</h1>
  <button v-if="!editing" @click="enableEdit(true)"> Modify </button>
  <button v-else @click="enableEdit(false)"> Cancel </button>
  <form v-if="editing" @submit.prevent="saveItem" >
    <input 
          type="text" 
          placeholder="Enter new item" 
          name="itemEntered"
          v-model="newItem" >

    <input 
          type="checkbox" 
          v-model="newItemHighPriority"
    > High Priority?

    <button :disabled="newItem.length==0" > Add Item</button>
  </form>

  <ul v-for="(item, key) in items" :key="item.id" >
    <li class="static-class1" 
        :class="[ 
                 item.highPriority ? 'priority' : 'regular',
                 item.purchased ? 'purchased' : '']"
        @click="togglePurchased(key)"
    > 
        {{key+1 + "."}} {{item.item }} 
    </li>
    
  </ul>
  <p v-if="!items.length">Shopping list is empty!</p>
 
</template>

 