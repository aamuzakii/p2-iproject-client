<template>
  <div class="home">
    <Nav></Nav>
    <Bar></Bar>

    <div class="d-flex flex-column home-container"  >
        
        <!--MDB Tables-->
        <div class="container">

            <div class="text-center darken-grey-text mb-4">
                <a class="btn btn-danger btn-md"
                @click.prevent="download"
                
                >Download as PDF<i class="fa fa-download pl-2"></i></a>
            </div>

            <div class="card mb-4">
                <div class="card-body">
                    <!-- Grid row -->
                    <div class="row">
                        <!-- Grid column -->
                        <div class="col-md-12">
                            <h2 class="pt-3 pb-4 text-center font-bold font-up deep-purple-text">Your Meal Intake Records</h2>
                            <div class="input-group md-form form-sm form-2 pl-0">
                                <input class="form-control my-0 py-1 pl-3 purple-border" type="text" placeholder="Search something here..." aria-label="Search">
                                <span class="input-group-addon waves-effect purple lighten-2" id="basic-addon1"><a><i class="fa fa-search white-text" aria-hidden="true"></i></a></span>
                            </div>
                        </div>
                        <!-- Grid column -->
                    </div>
                    <!-- Grid row -->
                    <!--Table-->
                    <table class="table table-striped" id="my-table">
                        <!--Table head-->
                        <thead>
                            <tr>
                                <th>#</th>
                                <th>Date</th>
                                <th>Food Name</th>
                                <th>Protein</th>
                                <th>Energy</th>
                                <th>Fat</th>
                                <th>Cholesterol</th>
                                <th>Carbohydrate</th>
                                <th>Sugars</th>
                            </tr>
                        </thead>
                        <!--Table head-->
                        <!--Table body-->
                        <tbody>
                            <Row
                            v-for="(row, index) in dietLog" :key="row.id" :row="row"
                            :index="index"
                            />
                        </tbody>
                        <!--Table body-->
                    </table>
                    <!--Table-->
                </div>
            </div>

            <hr class="my-4">
          
            <!-- <div class="text-center darken-grey-text mb-4">
                <h3 class="font-bold mb-3">Curious How Its Look Like?</h3>
                <a class="btn btn-danger"
                
                >Visualize</a>
            </div> -->

        </div>
        <!--MDB Tables-->
      <div id="page" class="mb-5" >
        <img class="w-100 mb-5 pb-5" :src="chartURL" alt="">
      </div>
    </div>

  </div>
</template>

<script>
import Nav from '../components/Nav.vue'
import Bar from '../components/Bar.vue'
import Row from '../components/Row.vue'
import Chart from '../components/Chart.vue'
import SelectCard from '../components/SelectCard.vue'
import { mapState } from 'vuex'
import { jsPDF } from "jspdf";
import html2canvas from 'html2canvas';
import 'jspdf-autotable'
import html2PDF from 'jspdf-html2canvas';

export default {
  name: "Diet",
  components : { Nav, Bar, SelectCard, Row, Chart },
  data() {
    return  {
    }
  },
  methods : {
    download() {
      const doc = new jsPDF({
        orientation: "landscape"
      });
      doc.autoTable({ html: '#my-table' })
      doc.text("Your Meal Intake Record", 120, 10);
      doc.save('table.pdf')

      // html2PDF(document.getElementById('page'), {
      //   jsPDF: {
      //     format: 'a4',
      //   },
      //   imageType: 'image/jpeg',
      //   output: './pdf/generate.pdf'
      // })
    },
  },
  created() {
    this.$store.dispatch("getChart")
    this.$store.dispatch("fetchDiet")
  },
  computed: {
  localComputed () { 
    return null
    /* ... */ },
  // mix this into the outer object with the object spread operator
  ...mapState({
    // ...
    dietLog : 'dietLog',
    chartURL : 'chartURL'
  })
}
}
</script>

<style scoped>

  div.card-body {
    overflow-x: auto;
  }

  #diet-input {
    border-radius: 20px;
  }

  .home-container {
    display: flex;
    flex-direction: column;
    position: absolute;
    top: 200px;
    left: 25vw;
    height: 70vh;
    width: 70vw;
    align-items: center;
  }
    /* justify-content: center; */

  .card-container-diet {
    position: absolute;
    top: 200px;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    flex-wrap: wrap ;
    height: 45vh;
    overflow: auto;
  }

  .page-nav{
    position: absolute;
    top: 170px;
    right: 50px;
  }

  .home {
    display: flex;
    flex-direction: column;
  }




  .hm-gradient {
    background-image: linear-gradient(to top, #f3e7e9 0%, #e3eeff 99%, #e3eeff 100%);
}
.darken-grey-text {
    color: #2E2E2E;
}
.input-group.md-form.form-sm.form-2 input {
    border: 1px solid #bdbdbd;
    border-top-left-radius: 0.25rem;
    border-bottom-left-radius: 0.25rem;
}
.input-group.md-form.form-sm.form-2 input.purple-border {
    border: 1px solid #9e9e9e;
}
.input-group.md-form.form-sm.form-2 input[type=text]:focus:not([readonly]).purple-border {
    border: 1px solid #ba68c8;
    box-shadow: none;
}
.form-2 .input-group-addon {
    border: 1px solid #ba68c8;
}
.danger-text {
    color: #ff3547; 
}  
.success-text {
    color: #00C851; 
}
.table-bordered.red-border, .table-bordered.red-border th, .table-bordered.red-border td {
    border: 1px solid #ff3547!important;
}        
.table.table-bordered th {
    text-align: center;
}

</style>
