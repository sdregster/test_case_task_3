<template>
  <div id="app">
    <div>
      <my-select
        v-model="selectedSortCity"
        :options="citySortOption"
        name="Выберите город"
      />
      <my-select
        v-model="selectedSortDepartment"
        :options="departmentSortOption"
        name="Выберите цех"
      />
      <my-select
        v-model="selectedSortWorker"
        :options="workersSortOption"
        name="Выберите сотрудника"
      />
    </div>
    <div>
      <table class="table">
        <thead>
          <th>Город</th>
          <th>Цех</th>
          <th>Сотрудник</th>
          <th>Бригада</th>
          <th>Смена</th>
        </thead>
        <tbody v-for="user in sortedData">
          <td>{{ user.city }}</td>
          <td>{{ user.department}}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.brigade }}</td>
          <td>{{ user.work_shift }}</td>
        </tbody>
      </table>
    </div>
  </div>
</template>


<script>
  import MySelect from './components/UI/MySelect.vue';
  import axios from 'axios';

  export default {
    components: {
      MySelect
    },
    data() {
      return {
        users: [],
        selectedSortCity: '',
        selectedSortDepartment: '',
        selectedSortWorker: '',
        citySortOption: [
          {value: 'Москва', name: 'Москва'},
          {value: 'Новокузнецк', name: 'Новокузнецк'},
        ],
        departmentSortOption: [
          {value: 'Ремонтный', name: 'Ремонтный'},
          {value: 'Кузовной', name: 'Кузовной'},
          {value: 'Сборочный', name: 'Сборочный'},
        ],
        workersSortOption: []
      }
    },
    methods: {
      async fetchUsers() {
        try {
          const response = await axios.get('http://localhost:3000/users');
          this.users = response.data;
        } catch (e) {
          console.log(e)
        }
      },
      setCookie(select1, select2, select3) {
          const cookie_object = new Object();
          cookie_object.selectedSortCity = select1;
          cookie_object.selectedSortDepartment = select2;
          cookie_object.selectedSortWorker = select3;
          document.cookie = ('cookie_object='+JSON.stringify(cookie_object));
      },
      getCookie() {
        const raw = document.cookie.split('=');
        const cookie_object = JSON.parse(raw[1]);
        this.selectedSortCity = cookie_object.selectedSortCity;
        this.selectedSortDepartment = cookie_object.selectedSortDepartment;
        this.selectedSortWorker = cookie_object.selectedSortWorker;
      }
    },
    mounted () {
      this.getCookie();
      this.fetchUsers();
    },
    computed: {
      sortedData() {
        // Все поля пустые
        if (this.selectedSortCity === '' && this.selectedSortDepartment === '' && this.selectedSortWorker === '') {
          const current_array = [...this.users];
          const cities_array = [...new Set(current_array.map(el => el.city))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.citySortOption = cities_array;
          const department_arr = [...new Set(current_array.map(el => el.department))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.departmentSortOption = department_arr;
          const workers_arr = [...new Set(current_array.map(el => el.name))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.workersSortOption = workers_arr;
          return current_array
        } 
        // Выбран только город
        else if (this.selectedSortCity !== '' && this.selectedSortDepartment === '' && this.selectedSortWorker === '') {
          const current_array = [...this.users].filter(user => user.city == this.selectedSortCity);
          const cities_array = [...new Set(current_array.map(el => el.city))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.citySortOption = cities_array;
          const department_arr = [...new Set(current_array.map(el => el.department))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.departmentSortOption = department_arr;
          const workers_arr = [...new Set(current_array.map(el => el.name))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.workersSortOption = workers_arr;
          this.setCookie(this.selectedSortCity, this.selectedSortDepartment, this.selectedSortWorker)
          return current_array
        }
        // Выбран только цех
        else if (this.selectedSortCity === '' && this.selectedSortDepartment != '' && this.selectedSortWorker === '') {
          const current_array = [...this.users].filter(user => user.department == this.selectedSortDepartment);
          const cities_array = [...new Set(current_array.map(el => el.city))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.citySortOption = cities_array;
          const department_arr = [...new Set(current_array.map(el => el.department))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.departmentSortOption = department_arr;
          const workers_arr = [...new Set(current_array.map(el => el.name))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.workersSortOption = workers_arr;
          this.setCookie(this.selectedSortCity, this.selectedSortDepartment, this.selectedSortWorker)
          return current_array
        } 
        // Выбран город и цех
        else if (this.selectedSortCity !== '' && this.selectedSortDepartment != '' && this.selectedSortWorker === '') {
          const current_array = [...this.users].filter(user => user.city == this.selectedSortCity).filter(user => user.department == this.selectedSortDepartment);
          const cities_array = [...new Set(current_array.map(el => el.city))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.citySortOption = cities_array;
          const department_arr = [...new Set(current_array.map(el => el.department))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.departmentSortOption = department_arr;
          const workers_arr = [...new Set(current_array.map(el => el.name))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.workersSortOption = workers_arr;
          this.setCookie(this.selectedSortCity, this.selectedSortDepartment, this.selectedSortWorker)
          return current_array
        } 
        // Выбраны все пункты
        else if (this.selectedSortCity !== '' && this.selectedSortDepartment != '' && this.selectedSortWorker !== '') {
          const current_array = [...this.users].filter(user => user.city == this.selectedSortCity).filter(user => user.department == this.selectedSortDepartment)
          .filter(user => user.name == this.selectedSortWorker);
          const cities_array = [...new Set(current_array.map(el => el.city))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.citySortOption = cities_array;
          const department_arr = [...new Set(current_array.map(el => el.department))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.departmentSortOption = department_arr;
          const workers_arr = [...new Set(current_array.map(el => el.name))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.workersSortOption = workers_arr;
          this.setCookie(this.selectedSortCity, this.selectedSortDepartment, this.selectedSortWorker)
          return current_array
        }
        // Выбран только сотрудник
        else if (this.selectedSortCity == '' && this.selectedSortDepartment == '' && this.selectedSortWorker !== '') {
          const current_array = [...this.users].filter(user => user.name == this.selectedSortWorker);
          const cities_array = [...new Set(current_array.map(el => el.city))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.citySortOption = cities_array;
          const department_arr = [...new Set(current_array.map(el => el.department))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.departmentSortOption = department_arr;
          const workers_arr = [...new Set(current_array.map(el => el.name))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.workersSortOption = workers_arr;
          this.setCookie(this.selectedSortCity, this.selectedSortDepartment, this.selectedSortWorker)
          return current_array
        } 
        // Выбран цех и сотрудник
        else if (this.selectedSortCity == '' && this.selectedSortDepartment !== '' && this.selectedSortWorker !== '') {
          const current_array = [...this.users].filter(user => user.department == this.selectedSortDepartment).filter(user => user.name == this.selectedSortWorker);
          const cities_array = [...new Set(current_array.map(el => el.city))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.citySortOption = cities_array;
          const department_arr = [...new Set(current_array.map(el => el.department))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.departmentSortOption = department_arr;
          const workers_arr = [...new Set(current_array.map(el => el.name))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.workersSortOption = workers_arr;
          this.setCookie(this.selectedSortCity, this.selectedSortDepartment, this.selectedSortWorker)
          return current_array
        } 
        // Выбран город и сотрудник
        else {
          const current_array = [...this.users].filter(user => user.city == this.selectedSortCity).filter(user => user.name == this.selectedSortWorker);
          const cities_array = [...new Set(current_array.map(el => el.city))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.citySortOption = cities_array;
          const department_arr = [...new Set(current_array.map(el => el.department))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.departmentSortOption = department_arr;
          const workers_arr = [...new Set(current_array.map(el => el.name))].map(el => {
            const o = new Object();
            o.value = el;
            o.name = el;
            return o
          });
          this.workersSortOption = workers_arr;
          this.setCookie(this.selectedSortCity, this.selectedSortDepartment, this.selectedSortWorker)
          return current_array
        }
      }
    },
  }
  </script>


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 5px;
}

.table {
	width: 100%;
  margin-top: 5px;
	margin-bottom: 5px;
	border: 2px solid #dddddd;
	border-collapse: collapse; 
}
.table th {
	font-weight: bold;
	padding: 5px;
	background: rgb(161, 161, 161);
	border: 2px solid #dddddd;
}
.table td {
	border: 1px solid #dddddd;
	padding: 5px;
}
</style>
