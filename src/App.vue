<template>
  <div id="app" class="container"><br>
  <h3 style="text-align: center; padding: 50px;">Selamat Datang Di Perpustakaan Online</h3>
    <div id="head">
      <div>
        <h4 class="card-title" style="font-family: cursive;">Form Peminjaman Buku</h4>
      </div>
        <div>
          <form @submit.prevent="add">
            <input type="hidden" v-model="form.id" required><h6>No :</h6>
            <label for="no"></label>
            <input type="text" v-model="form.no" required><br><br><h6>Nama mu:</h6>
            <label for="namaSiswa"></label>
            <input type="text" v-model="form.namaSiswa" required><br><br><h6>Judul Buku yang Kamu Pinjam :</h6>
            <label for="judulBuku"></label>
            <input type="text" v-model="form.judulBuku" required><br><br><h6>Tanggal Pinjam :</h6>
            <label for="tglPnjm"></label>
            <input type="text" v-model="form.tglPnjm" required><br><br><h6>Tanggal Pengembalian :</h6>
            <label for="tglKmbli"></label>
            <input type="text" v-model="form.tglKmbli" required><br><br>
            <button type="submit" v-show="!updateSubmit">Tambahkan Pinjaman Bukumu</button>
            <button type="button" v-show="updateSubmit" @click="update(form)">Perbarui</button>
          </form>
          </div>
          <div id="body">
            <h4>Tabel Daftar Buku</h4><br>
              <div id="a">
              <table>
                <thead>
                  <tr>
                    <th id="no">No</th>
                    <th id="ns">Nama Peminjam</th>
                    <th>Judul Buku Yang di Pinjam</th>
                    <th>Tanggal Pinjam</th>
                    <th>Tanggal Pengembalian</th>
                    <th>Perpanjang/Sudah dikembalikan</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="user,index in users" :key="index.id">
                    <td>{{ user.no }}</td>
                    <td>{{ user.namaSiswa }}</td>
                    <td>{{ user.judulBuku }}</td>
                    <td>{{ user.tglPnjm }}</td>
                    <td>{{ user.tglKmbli }}</td>
                    <td><button @click="edit(user)" id="ed">Perpanjang</button>
                    <button @click="del(user)" id="del">Kembali</button></td>
                  </tr>
                </tbody>
              </table>
              </div>
          </div>
        </div>
    </div>
</template>
 
<script>
/* eslint-disable */
import axios from 'axios'
export default {
  data () {
    return{
      form: {
        id: '',
        no: '',
        namaSiswa: '',
        judulBuku: '',
        tglPnjm: '',
        tglKmbli: '',
      },
      users: '',
      updateSubmit: false
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load(){
      axios.get('http://localhost:3000/users').then(res => {
        this.users = res.data 
      }).catch ((err) => {
        console.log(err);
      })
    },
    add(){
      axios.post('http://localhost:3000/users', this.form).then(res => {
        this.load()
        this.form.no = '',
        this.form.namaSiswa = '',
        this.form.judulBuku = '',
        this.form.tglPnjm = '',
        this.form.tglKmbli = ''
      })
    },
    edit(user){ 
        this.updateSubmit = true
        this.form.id = user.id 
        this.form.no = user.no 
        this.form.namaSiswa = user.namaSiswa 
        this.form.judulBuku = user.judulBuku 
        this.form.tglPnjm = user.tglPnjm 
        this.form.tglKmbli = user.tglKmbli 
    },
    update(form){ 
       return axios.put('http://localhost:3000/users/' + form.id , {no: this.form.no, namaSiswa: this.form.namaSiswa, judulBuku: this.form.judulBuku, tglPnjm: this.form.tglPnjm, tglKmbli: this.form.tglKmbli}).then(res => {
        this.load()
        this.form.id = '',
        this.form.no = '',
        this.form.namaSiswa = '',
        this.form.judulBuku = '',
        this.form.tglPnjm = '',
        this.form.tglKmbli = '',
        this.updateSubmit = false
      }).catch((err) => {
        console.log(err);
        
      })
    },
    del(user){
        axios.delete('http://localhost:3000/users/' + user.id).then(res =>{
            this.load()
            let index = this.users.indexOf(form.namaSiswa)
            this.users.splice(index,1)
      })
    }
  }
}
</script>

<style>
#app {
  background-color: rgb(184, 180, 180);
  height: 700px;
  width: 1325px;
  font-family: cursive;
}
tbody {
  height: 20px;
  width: 10px;
  background-color: rgb(218, 215, 207);
  border-top: 2px solid rgb(65, 64, 64);
  border-bottom: 2px solid rgb(65, 64, 64);
}
#body {
  position: relative;
  left: 350px;
  bottom: 447px;
}
#a {
  text-align: center;
}
table {
  border: 0px solid black;
  width: 760px;
  height: auto;
}
th {
  height: 7px;
  width: 100px;
  font-size: 15px;
  border: 1px solid rgb(32, 15, 2);
}
#no {
  width: 5px;
}
#ed {
  width: 100px;
  background-color: rgb(21, 197, 21);
  margin-bottom: 10px;
}
#del {
  width: 100px;
  background-color: red;
}
button {
  width: 300px;
  background-color: rgb(134, 131, 131);
  border-radius: 3px;
  border: hidden;
  color: white;
}
input {
  width: 300px;
  border-radius: 3px;
  border: hidden;
}
</style>