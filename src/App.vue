<script setup>
import { ref,computed  } from 'vue';



//variable drinking
const travelList=ref([
  {name: 'ร้านFleur Cafe & Eatery', price:100, img:"https://cdn.discordapp.com/attachments/1004965415870546035/1152259252400955473/1.webp",quantity: 0  }, 

  {name:'ร้านWTF Coffee Camp', price:180, img:"https://cdn.discordapp.com/attachments/1004965415870546035/1152259265407496213/2.webp" },
  
  {name:'ร้านElely Cafe’ Mae Ping Bistro',      price:150, img:"https://cdn.discordapp.com/attachments/1004965415870546035/1152259323662180362/3.webp" },

  {name:'ร้านอุตสาชงที่บ้าน', price:120, img:"https://cdn.discordapp.com/attachments/1004965415870546035/1152259331283234877/4-6.jpg.webp" },

  {name:'ร้านบ้านผ่อ-ดอย กอย-ดาว ร้านกาแฟกลางทุ่ง',    price:190, img:"https://cdn.discordapp.com/attachments/1004965415870546035/1152260535648264213/5_800x598.jpg.webp"},


  {name:'ร้านสุขพอดี Simply Happy', price:99,  img:"https://cdn.discordapp.com/attachments/1004965415870546035/1152259345560641626/6.webp" },


  {name:'ร้านChom Cafe and Restaurant', price:199, img:"https://cdn.discordapp.com/attachments/1004965415870546035/1152259356809760768/8.webp" },


  {name:' ร้านArtisan cafe', price:179, img:"https://cdn.discordapp.com/attachments/1004965415870546035/1152261496064192632/8-1_800x600-10.jpg.webp"  },
])


//เก็บ list การจอง 
const list_local = ref([


]);


//function
function list_local_control(name, quantity) {
  const existingItemIndex = list_local.value.findIndex(item => item.name === name);

  if (existingItemIndex !== -1) {
    list_local.value[existingItemIndex].quantity += quantity;
    list_local.value[existingItemIndex].price = list_local.value[existingItemIndex].quantity * travelList.value.find(item => item.name === name).price;
  } else {
    const item = {
      name: name,
      quantity: quantity,
      price: quantity * travelList.value.find(item => item.name === name).price
    };
    list_local.value.push(item);
  }
}


function incrementQuantity(item) {
  item.quantity++;
  item.price = item.quantity * travelList.value.find((travel) => travel.name === item.name).price;
}

function decrementQuantity(item) {
  if (item.quantity > 0) {
    item.quantity--;
    item.price = item.quantity * travelList.value.find((travel) => travel.name === item.name).price;
  }
}

function removeItem(index) {
  list_local.value.splice(index, 1);
}



//pop up

const form_costumer = ref({
  name: "",
  phone: "",
  date: "",
  time: ""
});

const showPopup = ref(false);

const totalCost = computed(() => {
    return list_local.value.reduce((acc, item) => acc + item.price, 0);
});


function togglePopup() {
  showPopup.value = !showPopup.value;
}


</script>



<template>

  <HelloWorld v-if="showPopup" />
  <div class="overlay" v-if="showPopup">
    <HelloWorld />
  </div>

  <div class="video-background">
        <video autoplay loop muted>
          <source src="https://cdn.discordapp.com/attachments/1004965415870546035/1152200148336779304/Dark_Aesthetic_Modern_Grand_Opening_Cafe_Mobile_Video.mp4" type="video/mp4">
        </video>
    </div>
  
  

  <div class="header">
    <img src="https://cdn.discordapp.com/attachments/1004965415870546035/1152218450199072778/cof-2.webp" class="circle">
    <H1>บริการจองโต๊ะ</H1>
    

  </div><hr>
  <h2>cafe รอบเชียงใหม่</h2><br>


  <div class="container text-center">
    <div class="row">
      <div class="col" v-for="(i,index) in travelList" :key="index">
        <div class="card" style="width: 18rem;" >
          <img :src="i.img" class="card-img-top">
          <div class="card-body">
            <h3 class="card-title">{{ i.name }}</h3>
            <p>ราคาโต๊ะ : {{i.price}}</p>
            <h6>รายละเอียดร้าน สินค้าและบริการ</h6>
            <p class="card-text">จำนวนโต๊ะ 
               <input type="number" class="cout_value " v-model="i.quantity">
            </p>
            <a class="btn btn-primary" @click="list_local_control(i.name, i.quantity)"> จองโต๊ะ </a>
          </div>
        </div>
      </div>
    </div>
  </div> 

 

  <h1 style="text-align: center; " v-if="list_local.length>0" class="header">รายการจองโต๊ะ</h1>
  <div class="list_cout text-center">
    <table class="table table-hover" v-if="list_local.length>0">
      <table class="table">
        <thead>
          <tr>
            <th scope="col">ลำดับ</th>
            <th scope="col">ชื่อร้าน</th>
            <th scope="col">จำนวนโต๊ะ</th>
            <th scope="col">ราคา</th>
            <th scope="col">//</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(i,index) in list_local" :key="index" >
            <th scope="row">{{index+1}}</th>
            <td>{{ i.name }}</td>
            <td>
              <div class="quantity-controls" >
                <span>{{ i.quantity }}</span>
              </div>
            </td>

            <td>{{ i.price}}</td>

            <td>
              <button type="button" class="btn btn-danger" @click="removeItem(index)" >ลบรายการ</button>
            </td>
          </tr>
        </tbody>
      </table>  
    </table>
  </div>  

  
  <div class="contrainer_costumer " v-if="list_local.length>0">
  <div class="form_box"> <h2 style="text-align: center;">กรุณากรอกข้อมูล</h2></div>
  <form >
    <div class="form_box">
      <label for="name">ชื่อ(ชื่อเล่นก็ได้) </label><br>
      <input type="text" id="name" class="form-control" v-model="form_costumer.name" required>
    </div>
    <div class="form_box">
      <label for="phone">เบอร์โทร </label><br>
      <input type="tel" id="phone" class="form-control" v-model="form_costumer.phone" required>
    </div>
    <div class="form_box">
      <label for="date">วันที่ </label><br>
      <input type="date" id="date" class="form-control" v-model="form_costumer.date" required>
    </div>
    <div class="form_box">
      <label for="time">เวลา </label><br>
      <input type="time" id="time" class="form-control" v-model="form_costumer.time" required>
    </div>
    <div class="form_box">
      <button type="button" class="btn btn-success" @click="togglePopup"> ยืนยันการจอง </button>
    </div>
  </form>
</div>


<div v-if="showPopup" class="overlay">

  <div class="popup">
      <div class="card-icon"> <img src="https://cdn.discordapp.com/attachments/1004965415870546035/1152250718103797811/376630671_10224393738618010_1789880118244281391_n.jpg" alt="" srcset=""> </div>
      <h2 style="text-align: center; color:green ;">ขอขอบคุณที่ใช้บริการ</h2>
      <h3 >รายละเอียด</h3>
      <p><strong>ชื่อ-นามสกุล:</strong> {{ form_costumer.name }}</p>
      <p><strong>เบอร์โทร:</strong> {{ form_costumer.phone }}</p>
      <p><strong>วันที่:</strong> {{ form_costumer.date }}</p>
      <p><strong>เวลา:</strong> {{ form_costumer.time }}</p>
      
      <table class="box_table">
          <thead>
              <tr>
                  <th>ลำดับ</th>
                  <th>ชื่อร้าน</th>
                  <th>จำนวนโต๊ะ</th>
                  <th>ราคา</th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="(i, index) in list_local" :key="index">
                  <td>{{ index + 1 }}</td>
                  <td>{{ i.name }}</td>
                  <td>{{ i.quantity }}</td>
                  <td>{{ i.price }}  บาท </td>
              </tr>
          </tbody>
      </table>

      <p><strong>รวม:</strong> {{ totalCost }} บาท</p>

      <button class="btn btn-danger" @click="showPopup = false">ปิดหน้าต่าง</button>
  </div>
</div>



</template>

<style src="./style.css">

</style>


