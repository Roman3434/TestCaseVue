<template>
  <div>
    <form class="form">
      <div class="col-1">
      <p>Данные клиента</p>
      <input v-model="Lname" type="text" placeholder="Фамилия">
      <input v-model="Fname" type="text" placeholder="Имя">
      <input v-model="Mname" type="text" placeholder="Отчество">
      <input v-model="Birthday" type="date">
      <input v-model="Phone" type="number" placeholder="7(***)*******">
      <select v-model="Gender">
        <option value="Male">Муж</option>
        <option value="Female">Жен</option>
      </select>
      <select v-model="GroupClient" multiple>
        <option value="VIP">VIP</option>
        <option value="Problems">Проблемные</option>
        <option value="ОМС">ОМС</option>
      </select>
      <select v-model="GroupDoctor">
        <option value="Иванов">Иванов</option>
        <option value="Захаров">Захаров</option>
        <option value="Чернышева">Чернышева</option>
      </select>
      <label for="sms">Не отправлять смс <input v-model="Sms" id="sms" type="checkbox"></label>
      </div>
      <div class="col-1">
      <p>Адрес</p>
      <input v-model="Indexx" type="number" placeholder="Индекс">
      <input v-model="Country" type="text" placeholder="Страна">
      <input v-model="Area" type="text" placeholder="Область">
      <input v-model="City" type="text" placeholder="Город">
      <input v-model="Street" type="text" placeholder="Улица">
      <input v-model="NumberHome" type="number" placeholder="Дом">
      </div>
      <div class="col-1">
      <p>Паспорт</p>
      <select v-model="TypeDocument">
        <option  value="Паспорт">Паспорт</option>
        <option value="Свидетельство о рождении">Свидетельство о рождении</option>
        <option value="Водительское удостоверение">Водительское удостоверение</option>
      </select>
      <input v-model="Passport" type="text" placeholder="Серия и номер паспорта">
      <input v-model="ByWhom" type="text" placeholder="Кем выдан">
      <input v-model="DateOfIssue" type="date">
      <button @click="Send">Отправить</button>
      </div>
    </form>

    <div class="error">
      <p>{{ error }}</p>
    </div>
    <div v-if="users.length === 0" class="empty">
      <p>Список клиентов пуст</p>
    </div>
    <div v-else class="client">
      <table>
        <tr>
          <td>Фамилия</td><td>Имя</td><td>Отчество</td><td>Дата рождения</td><td>Телефон</td><td>Пол</td><td>Группа клиента</td><td>Группа докторов</td><td>Смс</td> <td>Индекс</td> <td>Страна</td> <td>Область</td> <td>Город</td> <td>Улица</td> <td>Дом</td> <td>Тип документа</td> <td>Серия и номер паспорта</td> <td>Кем выдан</td> <td>Дата выдачи</td>
        </tr>
        <tr v-for="(el, index) in users" :key="index">
          <td>{{ el.Lname }}</td><td>{{ el.Fname }}</td><td>{{ el.Mname }}</td><td>{{ el.Birthday }}</td><td>{{ el.Phone }}</td><td>{{ el.Gender }}</td><td>{{ el.GroupClient }}</td><td>{{ el.GroupDoctor }}</td> <td>{{ el.Sms }}</td><td>{{ el.Indexx }}</td><td>{{ el.Country }}</td><td>{{ el.Area }}</td><td>{{ el.City }}</td> <td>{{ el.Street }}</td><td>{{ el.NumberHome }}</td><td>{{ el.TypeDocument }}</td><td>{{ el.Passport }}</td><td>{{ el.ByWhom }}</td><td>{{ el.DateOfIssue }}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MyHome',
  data(){
    return{
      users: [],
      locationUsers: [],
      DatePass: [],
      Lname: null,
      Fname: null,
      Mname: null,
      Birthday: null,
      Phone: null,
      Gender: null,
      GroupClient: null,
      GroupDoctor: null,
      Sms: null,
      Indexx: null,
      Country: null,
      Area: null,
      City: null,
      Street: null,
      NumberHome: null,
      TypeDocument: null,
      Passport: null,
      ByWhom: null,
      DateOfIssue: null,
      error: null,
    }
  },
  methods:{
    Send(e) {
      e.preventDefault();

      const errors = {};

      if (!this.Lname) {
        errors.Lname = 'Имя не заполнено';
      } else if(!this.validateName(this.Lname)){
        errors.Lname = 'Пожалуйста, введите корректную фамилию'
      }
      if (!this.Fname) {
        errors.Fname = 'Имя не заполнено';
      } else if(!this.validateName(this.Fname)){
        errors.Fname = 'Пожалуйста, введите корректное имя'
      }
      if(!this.Mname){
        errors.Mname = 'Отчество не заполнено';
      }
      else if(!this.validateName(this.Mname)){
        errors.Mname = 'Пожалуйста, введите корректное отчество'
      }
      if(!this.Birthday){
        errors.Birthday = 'Дата рождения не заполнено';
      }

      if(!this.Phone){
        errors.Phone = 'Введите телефон';
      }else if(!this.validatePhone(this.Phone)){
        errors.name = 'Введите корректный номер телефона'
      }
      if(!this.Gender){
        errors.Gender = 'Гендер введи'
      }
      if(!this.GroupClient){
        errors.GroupClient = 'Не выбрана группа клиентов'
      }
      if(!this.GroupDoctor){
        errors.GroupDoctor = 'Не выбрана группа клиентов'
      }
      if(!this.Indexx){
        errors.Indexx = 'Не заполнен индекс'
      } else if(!this.validateIndex(this.Indexx)){
        errors.Indexx = 'Введите корректный индекс';
      }
      if(!this.Country){
        errors.Country = 'Введите страну'
      }
      if(!this.Area){
        errors.Area = 'Введите область'
      }
      if(!this.City){
        errors.City = 'Введите город'
      }
      if(!this.Street){
        errors.Street = 'Введите улицу'
      }
      if(!this.NumberHome){
        errors.NumberHome = 'Введите страну'
      }
      if(!this.TypeDocument){
        errors.TypeDocument = 'Введите страну'
      }
      if (!this.Passport) {
        errors.passport = 'Паспорт не заполнен';
      } else if (!this.validatePassport(this.Passport)) {
        errors.passport = 'Пожалуйста, введите корректную серию и номер паспорта';
      }
      if(!this.ByWhom){
        errors.ByWhom = 'Кем выдан бл@'
      }
      if(!this.DateOfIssue){
        errors.DateOfIssue = 'Введите страну'
      }

      if (Object.keys(errors).length > 0) {
        this.error = Object.values(errors)[0]; // Выводим только первую ошибку для простоты
        return;
      }

      this.error = '';
      this.users.push({
        Lname: this.Lname,
        Fname: this.Fname,
        Mname: this.Mname,
        Birthday: this.Birthday,
        Phone: this.Phone,
        Gender: this.Gender,
        GroupClient: this.GroupClient,
        GroupDoctor: this.GroupDoctor,
        Sms: this.Sms,
        Indexx: this.Indexx,
        Country: this.Country,
        Area: this.Area,
        City: this.City,
        Street: this.Street,
        NumberHome: this.NumberHome,
        TypeDocument: this.TypeDocument,
        Passport: this.Passport,
        ByWhom: this.ByWhom,
        DateOfIssue: this.DateOfIssue
      });
      this.Lname = '';
      this.Fname = '';
      this.Mname = '';
      this.Birthday = null;
      this.Phone = '';
      this.Gender = null;
      this.GroupClient = null;
      this.GroupDoctor = null;
      this.Sms = null;
      this.Indexx = null;
      this.Country = '';
      this.Area = '';
      this.City = '';
      this.Street = '';
      this.NumberHome = null;
      this.TypeDocument = null;
      this.Passport = '';
      this.ByWhom = '';
      this.DateOfIssue = null;

      // Очистка ошибок после успешного добавления данных клиента
      this.error = '';
    }
    ,

    validatePassport(passport) {
      const passportRegex = /^\d{4}\s?\d{6}$/;
      return passportRegex.test(passport);
    },
    validateName(name) {
      // Регулярное выражение для проверки имени: только буквы и дефисы, длиной от 2 до 30 символов
      const nameRegex = /^[a-zA-Zа-яА-ЯёЁ]+(?:-[a-zA-Zа-яА-ЯёЁ]+)?$/;
      return nameRegex.test(name);
    },
    validatePhone(phone) {
      // Регулярное выражение для проверки телефонного номера: 11 цифр, начиная с 7
      const phoneRegex = /^7\d{10}$/;
      return phoneRegex.test(phone);
    },
    validateIndex(index) {
      // Регулярное выражение для проверки индекса: 6 цифр
      const indexRegex = /^\d{6}$/;
      return indexRegex.test(index);
    }
  }
}
</script>

<style>
*{
  font-size: clamp(12px, calc(0.5em + 1vw), 14px);
  box-sizing: border-box;
}
.form {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 20px;
  max-width: 1000px;
}

.form p {
  width: 100%;
  font-weight: bold;
}

.form input,
.form select,
.form button {
  flex: 1;
  max-width: 250px;
  min-width: 220px;
}

.error{
  color:red;
  font-weight: bold;
  width: 100%;
  max-width: 500px;
  min-width: 220px;
}
.empty {
  width: 100%;
  max-width: 500px;
  min-width: 220px;
}
.empty{
  display: flex;
  flex-direction: column;
  gap:20px;
  width: 100%;
  max-width: 500px;
  min-width: 220px;
}
table {
  max-width: 500px;
  min-width: 220px;
  width: 100%;
  margin-bottom: 20px;
  border: 1px solid #dddddd;
  border-collapse: collapse;
}
table th {
  font-weight: bold;
  padding: 5px;
  background: #efefef;
  border: 1px solid #dddddd;
}
table td {
  border: 1px solid #dddddd;
  padding: 5px;
}
.col-1{
  width: 100%;
  max-width: 250px;
  min-width: 220px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}
</style>
