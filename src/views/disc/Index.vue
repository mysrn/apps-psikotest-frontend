<template>
  <div class="container">
    <div class="card">
      <h5 class="card-header">DISC Test</h5>
      <div class="card-body">
        <div class="row row-cols-1 row-cols-md-3 g-4">
          <div class="card col col-lg-4">
            <ul class="list-group list-group-flush">
              <li class="list-group-item">Nama : auth.user.name</li>
              <li class="list-group-item">Usia : </li>
              <li class="list-group-item">Jenis Kelamin : </li>
              <li class="list-group-item">Tanggal Test : </li>
              <li class="list-group-item">
                <button class="btn btn-primary"
                  @click="submit()">
                  submit
                </button>
              </li>
            </ul>
          </div>
          <p class="card-title col col-lg-8">
            <strong>INSTRUKSI</strong>
             : Setiap nomor di bawah ini memuat 4 
             (empat) kalimat. Tugas anda adalah : 
            <br>1. Beri tanda [x] pada kolom di bawah huruf  
            <strong>[P]</strong> di samping kalimat yang 
            <strong>PALING menggambarkan diri anda</strong>
            <br>2. Beri tanda [x] pada kolom di bawah huruf  
            <strong>[K]</strong> di samping kalimat yang 
            <strong>PALING TIDAK menggambarkan diri anda</strong>
            <br><span class="text-danger">
              <strong>PERHATIKAN : Setiap nomor hanya ada 1 (satu) 
                tanda [x] di bawah masing-masing kolom P dan K.
              </strong></span>
              <br><small class="text-primary">Jiak sudah terisi semua, 
                tekan <span class="badge bg-primary">submit</span> 
                untuk mengirim jawaban</small>
            </p>
        </div>
        <pre>
          {{answer}}
        </pre>
        <div class="row row-cols-1 row-cols-md-3 g-4 mt-2">
          <div class="col" v-for="(test, i) in question_test"
            :key="test.squence">
            <div class="card shadow">
              <div class="card-body">
                <table class="table table-bordered border-primary">
                  <thead>
                    <tr>
                      <th scope="col">#</th>
                      <th scope="col">P</th>
                      <th scope="col">K</th>
                      <th scope="col">Gambaran Diri</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <th scope="row" rowspan="4">{{test.squence}}</th>
                      <td><input type="checkbox" @click="describePersonality(1)"></td>
                      <td><input type="checkbox" @click="notDescribePersonality(1)"></td>
                      <td>
                        {{test.describe_personality_1}}
                      </td>
                    </tr>
                    <tr>
                      <td><input type="checkbox" @click="describePersonality(2)"></td>
                      <td><input type="checkbox" @click="notDescribePersonality(2)"></td>
                      <td>
                        {{test.describe_personality_2}}
                      </td>
                    </tr>
                    <tr>
                      <td><input type="checkbox" @click="describePersonality(3)"></td>
                      <td><input type="checkbox" @click="notDescribePersonality(3)"></td>
                      <td>
                        {{test.describe_personality_3}}
                      </td>
                    </tr>
                    <tr>
                      <td><input type="checkbox" @click="describePersonality(4)"></td>
                      <td><input type="checkbox" @click="notDescribePersonality(4)"></td>
                      <td>
                        {{test.describe_personality_4}}
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>            
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import {useAuthStore} from '../../stores/auth'
  import axios from 'axios'
  export default{
    data() {
      return {
        question_test: [],
        answer:{
          describe_personality: [],
          not_describe_personality: [],
        },
      }
    },
    mounted() {
      this.questionTest()
    },
    methods: {
      questionTest(){
        this.$axios.get('/disc-question-test')
        .then(ress => this.question_test = ress.data.data)
        .catch(err => console.log(err))
      },
      describePersonality(data){
        this.answer.describe_personality.push(data)
      },
      notDescribePersonality(data){
        this.answer.not_describe_personality.push(data)
      },
      submit(){
        console.log(this.answer)
        this.answer = {
          describe_personality: [],
          not_describe_personality: []
        }
      }
    },
    setup() {
      const auth = useAuthStore()
      window.stores = { auth }
      return {
        auth
      }
    },
  }
</script>