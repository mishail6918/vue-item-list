<template>
  <div class="container">
    <h1>Items list</h1>
    <div class="wrapper">
      <div class="list">
        <form @submit.prevent>
          <div v-for="(li, index) of list" class="list__block">
              <button @click="submenuShow">arrow</button>
            <label class="custom-checkbox pre-checked" v-if="li.preChecked">
              <input type="checkbox" value="value-1" v-model="li.checked" @click="childCheck(index)">
              <span>{{li.name}}</span>
            </label>
            <label class="custom-checkbox" v-else>
              <input type="checkbox" value="value-1" v-model="li.checked" @click="childCheck(index)">
              <span>{{li.name}}</span>
            </label>
<!--            <label>-->
<!--              <button @click="submenuShow">arrow</button>-->
<!--              <input type="checkbox" v-model="li.checked" @click="childCheck(index)"/>-->
<!--              <span>{{li.name}}</span>-->
<!--            </label>-->
            <div class="sublist">
              <label v-for="item of li.sublist">
                <input type="checkbox" v-model="item.checked" @change="parentInputDotted(index,item)"/>
                <span>{{item.name}}</span>
                <input type="number" v-model="item.count" minlength="1" class="countInput">
                <input type="color" v-model="item.color">
              </label>
            </div>
          </div>
        </form>
      </div>
      <div class="sort-list">
        <div class="list__block" v-for="(li, index) of list">
          <div class="input-field">
            <select v-model="li.filter" @change="filteredSq(index)">
              <option value="sort" selected="selected">Сортировать</option>
              <option value="random">Перемешать</option>
            </select>
            <label>Filter</label>
          </div>
          <h3>{{li.name}}</h3>
          <div class="square-wrapper" v-for="item of li.sublist">
            <div class="square"
                 v-for="sq in item.count"
                 v-bind:style="{'background-color': item.color}"
                 v-if="item.checked === true"
                 @click="deleteItem(item)"
            ></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      list: [
        {
          name: 'list1',
          checked: false,
          filter: null,
          preChecked: null,
          sublist: [
            {
              name: 'Item1',
              color: '#FF0000',
              count: 5,
              checked: false
            },
            {
              name: 'Item2',
              color: '#fff200',
              count: 10,
              checked: false
            },
            {
              name: 'Item3',
              color: '#23ad00',
              count: 40,
              checked: false
            },
            {
              name: 'Item4',
              color: '#0066fa',
              count: 3,
              checked: false
            },
          ]
        },
        {
          name: 'list2',
          checked: false,
          filter: null,
          preChecked: null,
          sublist: [
            {
              name: 'Item1',
              color: '#FF0000',
              count: 3,
              checked: false
            },
            {
              name: 'Item2',
              color: '#fff200',
              count: 3,
              checked: false
            },
            {
              name: 'Item3',
              color: '#23ad00',
              count: 10,
              checked: false
            },
            {
              name: 'Item4',
              color: '#0066fa',
              count: 0,
              checked: false
            },
          ]
        }
      ],
    }
  },
  methods: {
    submenuShow(event) {
      let subList = event.target.nextElementSibling.nextElementSibling;
      if (subList.classList.contains('active-sub')) {
        subList.classList.remove('active-sub');
      }
      else {
        subList.classList.add('active-sub');
      }
    },
    childCheck(index) {
      if (this.list[index]['checked'] !== true) {
        this.list[index]['sublist'].forEach((element) => {
          element.checked = true;
        });
      }
      else {
        this.list[index]['sublist'].forEach((element) => {
          element.checked = false;
        });
      }
    },
    filteredSq(index) {
      if (this.list[index].filter === 'random') {
        return this.list[index]['sublist'].filter(t => t.checked === true);
        //console.log(filtedAr);
      }
      console.log(this.list[index].filter);
    },
    deleteItem(item) {
      item.count = item.count - 1;
    },
    parentInputDotted(index, item) {
      if (item.checked) {
        this.list[index].preChecked = true;
      }
      else {
        this.list[index].preChecked = false;
      }
    }
  },
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  width: 1100px;
  margin: 0 auto;
  max-width: 100%;
}
.wrapper {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
}
.list {
  border: 1px solid black;
  padding: 10px;
  min-height: 300px;
  width: 50%;
  .list__block {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    .sublist {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      margin-left: 30px;
      opacity: 0;
      visibility: hidden;
      height: 0;
      transition: 0.3s;
      label {
        padding: 5px 0px;
        .countInput {
          width: 35px;
        }
      }
      span {
        margin: 0px 5px;
      }
    }
    .active-sub {
      opacity: 1;
      visibility: visible;
      height: auto;
    }
  }
}
.sort-list {
  width: 50%;
  border: 1px solid black;
  min-height: 320px;
  .square-wrapper {
    display: flex;
    align-items: center;
    padding: 10px 0px;
    flex-wrap: wrap;
  }
  .square {
    width: 15px;
    height: 15px;
    margin: 0px 5px;
  }
}

.custom-checkbox>input {
  position: absolute;
  z-index: -1;
  opacity: 0;
}

.custom-checkbox>span {
  display: inline-flex;
  align-items: center;
  user-select: none;
}

.custom-checkbox>span::before {
  content: '';
  display: inline-block;
  width: 1em;
  height: 1em;
  flex-shrink: 0;
  flex-grow: 0;
  border: 1px solid #adb5bd;
  border-radius: 0.25em;
  margin-right: 0.5em;
  background-repeat: no-repeat;
  background-position: center center;
  background-size: 50% 50%;
}

.custom-checkbox>input:not(:disabled):not(:checked)+span:hover::before {
  border-color: #b3d7ff;
}

.custom-checkbox>input:not(:disabled):active+span::before {
  background-color: #b3d7ff;
  border-color: #b3d7ff;
}

.custom-checkbox>input:focus+span::before {
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}

.custom-checkbox>input:focus:not(:checked)+span::before {
  border-color: #80bdff;
}

.custom-checkbox>input:checked+span::before {
  border-color: #0b76ef;
  background-color: #0b76ef;
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 8 8'%3e%3cpath fill='%23fff' d='M6.564.75l-3.59 3.612-1.538-1.55L0 4.26 2.974 7.25 8 2.193z'/%3e%3c/svg%3e");
}

.pre-checked > span::before {
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg'%3E%3Ccircle r='4' cx='7' cy='7' fill='black'/%3E%3C/svg%3E");
  background-color: #fff;
  background-size: contain;
}

.custom-checkbox>input:disabled+span::before {
  background-color: #e9ecef;
}
</style>
