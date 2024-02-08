<script>
  export default {
    data() {
      return {
        showSecondSVG: false,
        changeBackground: false,
        itemListStorage: JSON.parse(localStorage.getItem('itemList')) || [],
      };
    },
    methods: {
      toggleSecondSVG() {
        this.showSecondSVG = !this.showSecondSVG;
      },
      toggleBackground(){
        this.changeBackground = !this.changeBackground;
      },
      changeSVGList(index) {
        console.log(index);
        let itemList = JSON.parse(localStorage.getItem('itemList')) || [];
        if (index >= 0 && index < itemList.length) {
          let item = itemList[index];
          if (typeof item === 'object' && item.hasOwnProperty('booleanValue')) {
            item.booleanValue = !item.booleanValue;
            localStorage.setItem('itemList', JSON.stringify(itemList));
          }
          this.itemListStorage = JSON.parse(localStorage.getItem('itemList')) || [];
        }
      },
      deleteItem(index) {
        let itemList = JSON.parse(localStorage.getItem('itemList')) || [];

        if (index >= 0 && index < itemList.length) {
          itemList.splice(index, 1);
          localStorage.setItem('itemList', JSON.stringify(itemList));
        }
        this.itemListStorage = JSON.parse(localStorage.getItem('itemList')) || [];
      },
      editItem(event, index) {
        let itemList = JSON.parse(localStorage.getItem('itemList')) || [];
        if (index >= 0 && index < itemList.length) {
          let item = itemList[index];
          if (item.hasOwnProperty('text')) {
            item.text = event.target.value;
            localStorage.setItem('itemList', JSON.stringify(itemList));
          }
        }
        this.itemListStorage = JSON.parse(localStorage.getItem('itemList')) || [];
      },
      saveNewItem(text) {
        let itemList = localStorage.getItem('itemList');
        if (!itemList) {
          itemList = [];
        } else {
          itemList = JSON.parse(itemList);
        }

        const newItem = {
          text: text.target.value,
          booleanValue: !this.showSecondSVG
        };

        itemList.push(newItem);

        localStorage.setItem('itemList', JSON.stringify(itemList));
        this.itemListStorage = JSON.parse(localStorage.getItem('itemList')) || [];
        text.target.value = '';
        this.showSecondSVG = false;
      }
    },
    computed: {
      buttonStyles() {
        return {
          backgroundColor: this.changeBackground ? 'red' : 'initial',
        };
      }
    }
  };
</script>

<template>
  <div style="background-color: lightblue; padding: 20px; border-radius: 10px; height: 500px; overflow-y: scroll;  position: relative; width: 550px;">
    <input id="titleInput" class="titleInput" :style="{ backgroundColor: changeBackground ? 'lightskyblue' : 'lightblue', color: 'white', 
    fontWeight: 'bold', fontFamily: 'Verdana, Geneva, Tahoma, sans-serif', fontSize: '30px', borderRadius: '5px' }" 
    value="List Title" @focus="toggleBackground" @blur="toggleBackground">
      <p style="color: white;">Enter en los items para guardar la edicion*</p>
    <div v-for="(item, index) in itemListStorage" style="position: relative;">
      <div style="color: black; font-size: 18px; padding: 5px; font-weight: bold; width: 500px;"  v-if="item.booleanValue">
        <div id="changeIcon" style="display: flex; justify-content: start; align-items: center; gap: 10px; border: 2px;
        background-color: white; padding: 3px; border-radius: 3px; height:;">
          <button @click="changeSVGList(index)" style="all:unset">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 512 512">
              <path fill="none" stroke="black" stroke-miterlimit="10" stroke-width="50" d="M448 256c0-106-86-192-192-192S64 150 64 256s86 192 192 192s192-86 192-192Z"/>
              <path fill="none" stroke="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="32" d="M368 192L256.13 320l-47.95-48m-16.23 48L144 272m161.71-80l-51.55 59"/>
            </svg>
          </button>
          <input id="listInput" class="listInput" 
          :value="item.text" @keydown.enter="editItem($event,index)">
        </div>
        <button style="position: absolute; z-index: 10; top: 12px; right: 10px;" @click="deleteItem(index)">Delete</button>
      </div>
    </div>
    <br>
    <p style="font-size: 30px; color: white; font-weight: bold; ">Done</p>
    <div v-for="(item, index) in itemListStorage" style="position: relative;">
      <div style="color: black; font-size: 18px; padding: 5px; font-weight: bold; width: 500px;"  v-if="!item.booleanValue">
        <div id="changeIcon" style="display: flex; justify-content: start; align-items: center; gap: 10px; border: 2px;
        background-color: white; padding: 3px; border-radius: 3px; height:;">
          <button @click="changeSVGList(index)" style="all:unset">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 512 512">
              <path fill="none" stroke="black" stroke-miterlimit="10" stroke-width="50" d="M448 256c0-106-86-192-192-192S64 150 64 256s86 192 192 192s192-86 192-192Z"/>
              <path fill="none" stroke="black" stroke-linecap="round" stroke-linejoin="round" stroke-width="32" d="M368 192L256.13 320l-47.95-48m-16.23 48L144 272m161.71-80l-51.55 59"/>
            </svg>
          </button>
          <input id="listInput" class="listInput" 
          :value="item.text" @keydown.enter="editItem($event,index)">
        </div>
        <button style="position: absolute; z-index: 10; top: 12px; right: 10px;"  @click="deleteItem(index)">Delete</button>
      </div>
    </div>

    <br>
    <br>

    <div id="changeIcon" style="display: flex; justify-content: start; align-items: center; gap: 10px;
    background-color: lightskyblue; padding: 3px; border-radius: 3px; position: fixed; top: 700px; width: 500px; z-index: 100;">
      <button v-if="showSecondSVG" @click="toggleSecondSVG" style="all:unset">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 512 512">
          <path fill="none" stroke="black" stroke-miterlimit="10" stroke-width="50" d="M448 256c0-106-86-192-192-192S64 150 64 256s86 192 192 192s192-86 192-192Z"/>
          <path fill="none" stroke="black" stroke-linecap="round" stroke-linejoin="round" stroke-width="32" d="M368 192L256.13 320l-47.95-48m-16.23 48L144 272m161.71-80l-51.55 59"/>
        </svg>
      </button>
      <button v-else @click="toggleSecondSVG" style="all:unset">
        <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 512 512">
          <path fill="none" stroke="black" stroke-miterlimit="10" stroke-width="50" d="M448 256c0-106-86-192-192-192S64 150 64 256s86 192 192 192s192-86 192-192Z"/>
          <path fill="none" stroke="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="32" d="M368 192L256.13 320l-47.95-48m-16.23 48L144 272m161.71-80l-51.55 59"/>
        </svg>
      </button>
      <input id="newListInput" class="newListInput" style="color: white;" 
      placeholder="Add a new Item" @keydown.enter="saveNewItem($event)">
    </div>
  </div>
</template>

<style scoped>
  .titleInput{
    all: unset; 
    padding: 5px;
  }
  .titleInput:hover{
    background-color: lightskyblue;
  }
  .titleInput:hover{
    background-color: lightskyblue;
  }
  .listInput{
    border: unset;
    outline: unset;
    background-color: unset;
    width: 350px;
  }
  .newListInput{
    border: unset;
    outline: unset;
    background-color: unset;
    color: white; 
    font-size: 18px;
  }
  .newListInput::placeholder {
    color: white;
  }
</style>
