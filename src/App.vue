<template>
    <div class="app-table" v-if="tableShow" v-on:dblclick="_onAppDblClick">
      <div v-if="showCntrl">
        <label for="qtRows">Строк:</label>
        <input type="text" name="" v-model="countRows" class="qtRows" placeholder="введите количество строк">
        <label for="qtCols">Столбцов:</label>
        <input type="text" name="" value="" v-model="countCols"  placeholder="введите количество столбцов">
        <textarea name="name" rows="5" cols="90" v-model="columns"></textarea>
        <button type="button" name="button" v-on:click="__createTable" >построить таблицу</button>
      </div>
      <div class="table-container" v-if="showBtn">
        <button type="button" name="button" v-on:click="__tableDelete">Удалить таблицу</button>
        <br>
        <button type="button" name="button"  v-on:click="__addRow" >Добавить строку в конец таблицы</button>
        <br>
        <input type="text" name="" value="" placeholder="введите индекс строки">
        <button type="button" name="button"  v-on:click="__insertRow">добавить строку в заданный индекс</button>
        <button type="button" name="button"v-on:click="__indexRowDelete">Удалить строку с заданным индексом</button>
        <br>
        <button type="button" name="button" v-on:click="__rowDelete">Удалить последнюю строку</button>
        <button type="button" name="button" v-on:click="__clearTable">Очистить таблицу</button>
        <button type="button" name="button" v-on:click="__getJSON">JSON выгрузка</button>
        <table>
          <thead>
            <tr>
              <th v-for="key in columns" @click="sortBy(key)">{{key}}</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(entry, i) in limitedItems ">
              <td v-for="key in columns">
                {{ entry[key] }}
              </td>
            </tr>
          </tbody>
        </table>
        <!-- <div v-if="showTr && startRow+rowsPerPage >= tableLen ">
          <span>пусто</span>
        </div> -->
        <div id="page-navigation" v-if="showPageNav">
          <button @click="movePages(-1)">Back</button>
          <p>{{startRow / rowsPerPage + 1}} out of {{Math.ceil(tableData.length / rowsPerPage) }}</p>
          <button @click="movePages(1)">Next</button>
        </div>
      </div>
      </div>
</template>

<script>
export default {
  name: 'app',

  data () {

    return {
      tableData: [],
      jsonData:`[
        {
          "_id": "59f7095655e5a19e327e094d",
          "index": 0,
          "guid": "d741ae6d-6c22-4885-be7d-263076679f08",
          "isActive": false,
          "balance": "$2,090.60",
          "picture": "http://placehold.it/32x32",
          "age": 35,
          "eyeColor": "blue",
          "name": {
            "first": "Shelia",
            "last": "Atkins"
          },
          "company": "POLARAX",
          "email": "shelia.atkins@polarax.co.uk",
          "phone": "+1 (991) 499-2956",
          "address": "927 Kenilworth Place, Bordelonville, New York, 2300",
          "about": "Ullamco anim consequat esse ex nisi. Magna eu duis irure consectetur reprehenderit est dolore consectetur ut sit tempor officia. Ea dolor ea fugiat ex adipisicing non eu qui. Aliquip ut pariatur minim aliqua excepteur commodo et exercitation mollit quis ut. Ipsum cupidatat sunt qui est voluptate adipisicing dolor laboris minim reprehenderit cupidatat cillum.",
          "registered": "Wednesday, July 5, 2017 5:56 PM",
          "latitude": "65.829355",
          "longitude": "-62.344585",
          "tags": [
            "anim",
            "laboris",
            "officia",
            "proident",
            "fugiat"
          ],
          "range": [
            0,
            1,
            2,
            3,
            4,
            5,
            6,
            7,
            8,
            9
          ],
          "friends": [
            {
              "id": 0,
              "name": "Deidre Giles"
            },
            {
              "id": 1,
              "name": "Marshall Carter"
            },
            {
              "id": 2,
              "name": "Sally Floyd"
            }
          ],
          "greeting": "Hello, Shelia! You have 6 unread messages.",
          "favoriteFruit": "strawberry"
        },
        {
          "_id": "59f7095665b55c06455d3cf2",
          "index": 1,
          "guid": "55d2d25a-93af-4f33-bb03-27831f367237",
          "isActive": false,
          "balance": "$2,973.97",
          "picture": "http://placehold.it/32x32",
          "age": 24,
          "eyeColor": "green",
          "name": {
            "first": "Tyler",
            "last": "Clements"
          },
          "company": "QUIZMO",
          "email": "tyler.clements@quizmo.us",
          "phone": "+1 (912) 497-3549",
          "address": "501 Duryea Place, Goodville, Montana, 7305",
          "about": "Deserunt excepteur esse proident adipisicing veniam. Est anim sunt reprehenderit ut enim commodo irure Lorem. Qui sunt irure irure consequat adipisicing ex proident eu ea commodo ut. Aliquip irure ex veniam proident ullamco veniam ipsum pariatur dolor irure. Dolor consectetur nostrud ut dolore mollit id reprehenderit id consequat magna.",
          "registered": "Tuesday, March 11, 2014 1:21 AM",
          "latitude": "-8.465862",
          "longitude": "143.939384",
          "tags": [
            "minim",
            "id",
            "duis",
            "occaecat",
            "labore"
          ],
          "range": [
            0,
            1,
            2,
            3,
            4,
            5,
            6,
            7,
            8,
            9
          ],
          "friends": [
            {
              "id": 0,
              "name": "Davenport Burch"
            },
            {
              "id": 1,
              "name": "Sheppard Hickman"
            },
            {
              "id": 2,
              "name": "Navarro Mcintosh"
            }
          ],
          "greeting": "Hello, Tyler! You have 6 unread messages.",
          "favoriteFruit": "apple"
        },
        {
          "_id": "59f70956e2f9f7c40ac00da5",
          "index": 2,
          "guid": "3c647acc-8dd4-454b-97fb-2adc17fb8938",
          "isActive": false,
          "balance": "$3,541.04",
          "picture": "http://placehold.it/32x32",
          "age": 27,
          "eyeColor": "blue",
          "name": {
            "first": "Kate",
            "last": "Golden"
          },
          "company": "NETPLODE",
          "email": "kate.golden@netplode.biz",
          "phone": "+1 (959) 497-2334",
          "address": "478 Kingsland Avenue, Retsof, District Of Columbia, 7906",
          "about": "Veniam anim ex minim officia excepteur officia cillum Lorem dolore fugiat velit. Et laboris cupidatat do commodo culpa occaecat Lorem enim ea tempor. Occaecat magna labore dolor ullamco incididunt amet duis et adipisicing. Occaecat fugiat excepteur cupidatat enim nulla id duis. Magna exercitation pariatur eiusmod veniam exercitation id fugiat elit ea. Aliqua deserunt qui consectetur in laboris.",
          "registered": "Thursday, September 28, 2017 11:51 PM",
          "latitude": "71.754172",
          "longitude": "96.070599",
          "tags": [
            "aute",
            "amet",
            "ad",
            "laborum",
            "voluptate"
          ],
          "range": [
            0,
            1,
            2,
            3,
            4,
            5,
            6,
            7,
            8,
            9
          ],
          "friends": [
            {
              "id": 0,
              "name": "Maude Dalton"
            },
            {
              "id": 1,
              "name": "Saunders Buck"
            },
            {
              "id": 2,
              "name": "Herman Reeves"
            }
          ],
          "greeting": "Hello, Kate! You have 9 unread messages.",
          "favoriteFruit": "strawberry"
        },
        {
          "_id": "59f70956bec2c369dda7ef74",
          "index": 3,
          "guid": "2e8ddd90-565f-462f-98fd-af8f54812f7a",
          "isActive": false,
          "balance": "$3,990.48",
          "picture": "http://placehold.it/32x32",
          "age": 38,
          "eyeColor": "blue",
          "name": {
            "first": "Mai",
            "last": "Kramer"
          },
          "company": "ATOMICA",
          "email": "mai.kramer@atomica.io",
          "phone": "+1 (997) 439-2800",
          "address": "687 Seeley Street, Titanic, Alaska, 5942",
          "about": "Sit ad officia reprehenderit ad ullamco magna. Aute do eu excepteur fugiat elit. Cillum nisi aliqua sint fugiat veniam id ipsum. Quis incididunt veniam cillum anim nostrud ullamco cillum magna. Fugiat ex ad eiusmod ea adipisicing quis in eiusmod. Nisi elit excepteur dolor exercitation. Non minim sunt adipisicing aute enim magna.",
          "registered": "Friday, October 27, 2017 9:59 AM",
          "latitude": "-85.017223",
          "longitude": "-156.709183",
          "tags": [
            "ut",
            "officia",
            "cillum",
            "eu",
            "pariatur"
          ],
          "range": [
            0,
            1,
            2,
            3,
            4,
            5,
            6,
            7,
            8,
            9
          ],
          "friends": [
            {
              "id": 0,
              "name": "Louise Pierce"
            },
            {
              "id": 1,
              "name": "Bobbi Ruiz"
            },
            {
              "id": 2,
              "name": "Medina Lane"
            }
          ],
          "greeting": "Hello, Mai! You have 5 unread messages.",
          "favoriteFruit": "banana"
        },
        {
          "_id": "59f7095619e60544f1b0cc9e",
          "index": 4,
          "guid": "ee65a6be-3670-4a37-b38d-0809b4ef15c1",
          "isActive": true,
          "balance": "$1,186.20",
          "picture": "http://placehold.it/32x32",
          "age": 25,
          "eyeColor": "green",
          "name": {
            "first": "Gena",
            "last": "Stephens"
          },
          "company": "PERKLE",
          "email": "gena.stephens@perkle.tv",
          "phone": "+1 (858) 593-2505",
          "address": "718 Veterans Avenue, Forestburg, Colorado, 1127",
          "about": "Magna officia exercitation sint nulla Lorem ex deserunt fugiat do aliqua sit ex cillum. Ipsum qui excepteur anim minim incididunt incididunt consectetur cillum reprehenderit ipsum. Commodo et non aliquip consequat mollit duis sint officia id sint magna reprehenderit consectetur aliqua. Occaecat id culpa quis tempor culpa sit. Dolore nisi anim cupidatat ullamco deserunt amet adipisicing excepteur eu non.",
          "registered": "Wednesday, November 25, 2015 8:18 AM",
          "latitude": "43.410091",
          "longitude": "-18.278446",
          "tags": [
            "pariatur",
            "ut",
            "sit",
            "sunt",
            "nostrud"
          ],
          "range": [
            0,
            1,
            2,
            3,
            4,
            5,
            6,
            7,
            8,
            9
          ],
          "friends": [
            {
              "id": 0,
              "name": "Dunlap Love"
            },
            {
              "id": 1,
              "name": "Sims Rollins"
            },
            {
              "id": 2,
              "name": "Florence Day"
            }
          ],
          "greeting": "Hello, Gena! You have 9 unread messages.",
          "favoriteFruit": "strawberry"
        },
        {
          "_id": "59f709567edb31677a027d5e",
          "index": 5,
          "guid": "fec98e01-bb81-4084-b03a-bb41697b8749",
          "isActive": false,
          "balance": "$3,447.96",
          "picture": "http://placehold.it/32x32",
          "age": 29,
          "eyeColor": "green",
          "name": {
            "first": "Susanna",
            "last": "Lang"
          },
          "company": "PROGENEX",
          "email": "susanna.lang@progenex.ca",
          "phone": "+1 (839) 585-2411",
          "address": "528 Bogart Street, Southmont, Maine, 7873",
          "about": "Commodo est non veniam mollit aliquip qui exercitation duis sunt incididunt dolore quis. Proident sint deserunt enim voluptate elit id qui ex. Duis dolor proident mollit eiusmod excepteur Lorem commodo veniam ad enim dolor voluptate.",
          "registered": "Wednesday, September 20, 2017 4:15 AM",
          "latitude": "14.22528",
          "longitude": "107.827014",
          "tags": [
            "non",
            "adipisicing",
            "elit",
            "in",
            "eiusmod"
          ],
          "range": [
            0,
            1,
            2,
            3,
            4,
            5,
            6,
            7,
            8,
            9
          ],
          "friends": [
            {
              "id": 0,
              "name": "Dominique Rutledge"
            },
            {
              "id": 1,
              "name": "Ellison Ellis"
            },
            {
              "id": 2,
              "name": "Alfreda Parsons"
            }
          ],
          "greeting": "Hello, Susanna! You have 6 unread messages.",
          "favoriteFruit": "strawberry"
        },
        {
          "_id": "59f70956d7ab97326652a619",
          "index": 6,
          "guid": "9def96dd-0791-4854-a6a6-578d860c8003",
          "isActive": true,
          "balance": "$3,836.95",
          "picture": "http://placehold.it/32x32",
          "age": 35,
          "eyeColor": "blue",
          "name": {
            "first": "Terry",
            "last": "West"
          },
          "company": "REMOTION",
          "email": "terry.west@remotion.info",
          "phone": "+1 (984) 499-2241",
          "address": "774 Montauk Avenue, Rosewood, Kentucky, 3956",
          "about": "Incididunt sit ut ut sunt reprehenderit aute mollit nisi qui amet id deserunt nisi. Incididunt laborum reprehenderit amet proident do officia adipisicing aliqua aliquip in est laboris veniam. Irure veniam velit consequat est reprehenderit minim sit ea deserunt nulla amet. Sint exercitation nisi sint deserunt Lorem non aliquip non ullamco.",
          "registered": "Monday, January 25, 2016 10:54 PM",
          "latitude": "48.57755",
          "longitude": "154.502634",
          "tags": [
            "tempor",
            "mollit",
            "minim",
            "qui",
            "commodo"
          ],
          "range": [
            0,
            1,
            2,
            3,
            4,
            5,
            6,
            7,
            8,
            9
          ],
          "friends": [
            {
              "id": 0,
              "name": "Nielsen Sampson"
            },
            {
              "id": 1,
              "name": "Silvia Case"
            },
            {
              "id": 2,
              "name": "Chandler Ray"
            }
          ],
          "greeting": "Hello, Terry! You have 9 unread messages.",
          "favoriteFruit": "apple"
        },
        {
          "_id": "59f7095607a6dc1fef8d98a6",
          "index": 7,
          "guid": "8f306c97-c2da-4cda-aae1-30a3b089ca1f",
          "isActive": false,
          "balance": "$1,603.49",
          "picture": "http://placehold.it/32x32",
          "age": 37,
          "eyeColor": "blue",
          "name": {
            "first": "Hall",
            "last": "Morton"
          },
          "company": "XYMONK",
          "email": "hall.morton@xymonk.biz",
          "phone": "+1 (800) 569-2690",
          "address": "913 Dictum Court, Whitehaven, Minnesota, 1021",
          "about": "Ad id aliquip voluptate eu enim in nostrud aliqua incididunt nostrud veniam amet quis consequat. Anim non proident eiusmod ullamco excepteur minim commodo sunt laborum aliqua magna nostrud incididunt cupidatat. Ut eu dolor voluptate fugiat qui incididunt culpa ea exercitation dolor sint do duis est. Ex et ad nostrud voluptate ullamco qui exercitation laborum ipsum ipsum est.",
          "registered": "Wednesday, May 24, 2017 4:07 PM",
          "latitude": "-71.990792",
          "longitude": "-102.113318",
          "tags": [
            "culpa",
            "commodo",
            "irure",
            "adipisicing",
            "tempor"
          ],
          "range": [
            0,
            1,
            2,
            3,
            4,
            5,
            6,
            7,
            8,
            9
          ],
          "friends": [
            {
              "id": 0,
              "name": "Maria Heath"
            },
            {
              "id": 1,
              "name": "Monique Simmons"
            },
            {
              "id": 2,
              "name": "Lourdes Ramirez"
            }
          ],
          "greeting": "Hello, Hall! You have 8 unread messages.",
          "favoriteFruit": "banana"
        },
        {
          "_id": "59f7095601981dd78d6369ee",
          "index": 8,
          "guid": "4b4f31d6-44e7-4449-be73-f0e1aabd3e45",
          "isActive": false,
          "balance": "$3,370.59",
          "picture": "http://placehold.it/32x32",
          "age": 26,
          "eyeColor": "green",
          "name": {
            "first": "Henrietta",
            "last": "Crane"
          },
          "company": "ELPRO",
          "email": "henrietta.crane@elpro.name",
          "phone": "+1 (985) 583-2352",
          "address": "702 Pierrepont Street, Oneida, Marshall Islands, 4035",
          "about": "Fugiat id magna aliqua do velit elit pariatur do velit cillum pariatur nisi ex. Magna deserunt ex amet amet mollit. Consequat in in aliqua culpa ipsum velit fugiat velit duis reprehenderit incididunt occaecat proident. Reprehenderit magna adipisicing labore in sunt cillum laboris. Reprehenderit do tempor Lorem exercitation occaecat qui dolore aute.",
          "registered": "Sunday, August 23, 2015 5:08 PM",
          "latitude": "-72.936756",
          "longitude": "-117.593618",
          "tags": [
            "exercitation",
            "consequat",
            "adipisicing",
            "ut",
            "consequat"
          ],
          "range": [
            0,
            1,
            2,
            3,
            4,
            5,
            6,
            7,
            8,
            9
          ],
          "friends": [
            {
              "id": 0,
              "name": "Sandy Caldwell"
            },
            {
              "id": 1,
              "name": "Chasity Howell"
            },
            {
              "id": 2,
              "name": "Deirdre Campos"
            }
          ],
          "greeting": "Hello, Henrietta! You have 5 unread messages.",
          "favoriteFruit": "apple"
        },
        {
          "_id": "59f7095639d9916db0a9d696",
          "index": 9,
          "guid": "6f93c5cc-7bc9-47ce-8cfa-555668bd4b0c",
          "isActive": false,
          "balance": "$3,278.82",
          "picture": "http://placehold.it/32x32",
          "age": 23,
          "eyeColor": "green",
          "name": {
            "first": "Katina",
            "last": "Wilson"
          },
          "company": "FLUM",
          "email": "katina.wilson@flum.me",
          "phone": "+1 (988) 424-2803",
          "address": "693 Monroe Street, Springville, Palau, 5854",
          "about": "Excepteur exercitation ullamco occaecat exercitation mollit elit ullamco nostrud cillum eiusmod aute ipsum amet nulla. Reprehenderit duis Lorem veniam consequat labore irure sunt labore nulla. Officia sint quis quis est Lorem esse elit laboris anim dolore occaecat ad voluptate. Irure duis aute quis minim cupidatat dolor irure aute pariatur. Deserunt ex officia elit elit exercitation commodo.",
          "registered": "Thursday, October 26, 2017 11:33 PM",
          "latitude": "-33.816044",
          "longitude": "43.213969",
          "tags": [
            "qui",
            "proident",
            "reprehenderit",
            "ex",
            "sint"
          ],
          "range": [
            0,
            1,
            2,
            3,
            4,
            5,
            6,
            7,
            8,
            9
          ],
          "friends": [
            {
              "id": 0,
              "name": "Annmarie Rowland"
            },
            {
              "id": 1,
              "name": "Casey Cruz"
            },
            {
              "id": 2,
              "name": "Cantu Vaughn"
            }
          ],
          "greeting": "Hello, Katina! You have 7 unread messages.",
          "favoriteFruit": "apple"
        }
      ]`,
      columns: "id, name, age, company, email, phone, address, registered, tags, friends, greeting, favoriteFruit",
      tableShow:true,
      showPageNav:true,
      showCntrl: true,
      showBtn: false,
      showTr: false,
      countRows: 0,
      countCols:0,
      tableLen: 1,
      sortKey: ' ',
      sortOrders: null,
      startRow: 0,
      rowsPerPage: 5,

    }
  },
  created() {
    this.tableData = JSON.parse(this.jsonData).map(item => {
        let x = ``;
        for (let i = 0; i < item.friends.length; i++) {
          x+=item.friends[i].name + ', '
        }
        return {
          id: item._id,
          name: `${item.name.first} ${item.name.last}`,
          age: item.age,
          company: item.company,
          email: item.email,
          phone: item.phone,
          address: item.address,
          registered: item.registered,
          tags:`${item.tags}`,
          friends: x,
          greeting: item.greeting,
          favoriteFruit: item.favoriteFruit,
        }
    });
  },
  computed: {
    row() {
      let obj = {};
      for (let key in this.tableData[0]) {
        obj[key] = '';
      }
      return obj;
    },

    filteredData: function () {
      var sortKey = this.sortKey;
      var order = this.sortOrders[sortKey] || 1;
      var data = this.tableData;
      if (sortKey) {
        data = data.slice().sort(function (a, b) {;
          a = a[sortKey];
          b = b[sortKey];
          return (a === b ? 0 : a > b ? 1 : -1) * order;
        })
      }
      return this.tableData = data;
    },

    limitedItems: function() {
      this.filteredData.length / this.rowsPerPage < 1
      ?this.showPageNav=false
      :this.showPageNav=true
      let items = this.tableData.slice(this.startRow, this.startRow + this.rowsPerPage);
      if (items.length == 1 ) {
        this.startRow -=  this.rowsPerPage;
      }
      return items;
    }
  },
  methods: {
    movePages: function(amount) {
      var newStartRow = this.startRow + (amount * this.rowsPerPage);
      if (newStartRow >= 0 && newStartRow < this.filteredData.length) {
        this.startRow = newStartRow;
       }
    },
    __createTable(){
      let rows = +this.countRows;
      let cols = +this.countCols;
      let tableData = this.tableData;
      let jsonData = JSON.parse(this.jsonData);
      this.tableLen = this.tableData.length;

      if(isNaN(rows) ||
      isNaN(cols) ||
      rows>50 ||
      cols>50||
      rows==0||
      cols==0){
        alert("Введены некорректные данные");
        this.countRows = 0;
        this.countCols = 0;
        return;
      }

      if (rows > this.tableLen) {
        for(let i = this.tableLen, len = rows; i < rows; i++ ){
          this.tableData.push(this.row);
        }
      }

      this.columns = this.columns.split(", ");

      if (this.columns.length < cols) {
        for (var i = this.columns.length; i < cols; i++) {
          this.columns[i]="";
        }
      }
      if (this.columns.length > cols){
        this.columns = this.columns.slice(0,cols);
      }

      let o = {};
      this.columns.forEach(key => {
        o[key] = 1;
      })
      this.sortOrders = o;

      this.showCntrl = false;
      this.showBtn = true;
    },
    __tableDelete(){
      if(confirm("Вы уверены что хотите удалить таблицу")){
        return this.tableShow=false;
      }
    },
    sortBy(key) {
        this.sortKey = key
        this.sortOrders[key] = this.sortOrders[key] * -1
      },
    __addRow(){
      this.tableData.push(this.row);
      console.log(this.filteredData);
      console.log(this.tableData);

    },
    __insertRow(){
      let val = +this.$el.querySelector("input").value;

      if (isNaN(val) ||
      (this.length < val)||
      val==0){
        alert("Введены некорректные данные");
        this.$el.querySelector("input").value="";
        return;
      }
      this.filteredData.splice(val-1, 0, this.row);
    },
    __rowDelete(){
      let len = this.filteredData.length;

      if (len==1) {
        alert("должна быть хотя бы одна строка");
        return;
      }
      this.filteredData.splice(len-1,1);
    },

    __indexRowDelete()  {
      let val = +this.$el.querySelector("input").value;
      let len = this.filteredData.length;

      if (isNaN(val) ||
      (len < val)||
      val==0){
        alert("Введены некорректные данные");
        this.$el.querySelector("input").value="";
        return;
      }
      if (len==1) {
        alert("должна быть хотя бы одна строка");
        return;
      }
      this.filteredData.splice(val-1, 1);
    },

    __clearTable(){
      let rows=this.$el.querySelectorAll("tbody >tr");

      HTMLCollection.prototype.clear = function() {
        for (let i = 0, len = this.length; i < len; i += 1) {
          this[i].innerHTML = '';
        }
      }
      for(let i = 0, len = rows.length; i < len; i += 1){
        rows[i].cells.clear();
      }
    },

    _onAppDblClick(e) {
      let td = e.target.closest('td');
      if (!td) {
        return;
      }
      let input = document.createElement('input');
      let value = td.textContent;

      td.innerHTML = '';
      td.appendChild(input);

      input.value = value;
      input.focus();

      input.onblur = function(e) {
				td.textContent = this.value;
			};
    },
    __getJSON(){
      let data = [];
					for (let i = 0, len=  this.$el.querySelector("tbody").children.length; i<len; i++) {
						let row = {};
						for (let j = 0, len1=  this.$el.querySelector("thead").children[0].children.length; j<len1; j++) {
							row[this.$el.querySelector("thead").children[0].children[j].innerHTML.trim()] = this.$el.querySelector("tbody").children[i].children[j].textContent.trim();
            }
						data.push(row);
					}
					console.log(JSON.stringify(data, null, 6));
    }
  }
}
</script>

<style lang="sass">
.app-table
  font-family: 'Avenir', Helvetica, Arial, sans-serif
  margin-top: 40px
  border: 1px solid black
  padding: 0
  border-radius: 10px
  input,button
    padding-left: 2px
    margin: 5px
    border-radius: 10px
    border: none
  button
    border: 1px solid black
  input
    background-color: #DCDCDC
  textarea
    max-width: 80%
    display: block
    border-radius: 5px

.table-container
  width: 100%
  margin: auto
  overflow-x: scroll

table
  border: 1px solid black
  border-collapse: collapse
  margin: 10px
  td, th
    padding: 0
    min-width: 100px
    height: 27px
    padding: 1px
    border-right: 1px solid silver
    border-bottom: 1px solid silver
    input
      margin: 0 !important
      padding: 0 !important
      width: 100px
  th
    border-radius: 5px
tbody
  tr:nth-child(even)
    background-color: #f3f3f3
  tr:nth-child(odd)
    background-color: #ddd
#page-navigation
  p
    float: left
  button
    height: 40px
    float: left

</style>
