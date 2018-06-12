<template>
    <div>
        <div class="box">
            <div class="level">
                <div class="level-left">
                    <div>
                        <div class="subtitle">Filter Your Search</div>
                        <div class="level-item">
                            <input v-if="searchSelectType =='3' "  class="input"  type="text" placeholder="Find a stock">
                            <div v-else-if="searchSelectType =='1'" class="select">
                                      <select v-model="selectedParty">
                                          <option  value="cash" selected>cash</option>
                                          <option v-for="account in party" :value="account">{{account.name}}</option>
                                      </select>
                                  </div>
                            <div class="select">
                                <select @change="searchSelectChanged($event)">
                                    <option  selected >All Party - All Period</option>
                                    <option >Party wise</option>
                                    <option >Period wise</option>
                                    <option >Invoice number</option>
                                </select>
                                
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <br/>
        <table class="table is-bordered is-striped is-fullwidth">
            <thead>
            <tr>
                <th>Serial no.</th>
                <th>Invoice No.</th>
                <th>Invoice Date</th>
                <th>Party Name</th>
                <th>GST NO</th>
                <th>Total Taxable Amount</th>
                <th>Total GST</th>
                <th>Total Amount</th>
            </tr>
            </thead>
            <tbody>
                <!-- "detail":{"invoice":"ANV17PR","date":"2018-04-11","party":"cash","totalAmount":128.57600000000002} -->
            <tr v-for="(row,i) in rows">
                <th>{{ i + 1 }}</th>
                <td>
                    {{ row.detail.invoice }}
                </td>
                <td>
                    {{ row.detail.date }}
                </td>
                <td>
                    {{ row.detail.party }}
                </td>
                <td>
                    {{row.detail.gstNo}}
                </td>
                <td>
                    {{row.detail.getTotalTaxableAmount}}
                </td>
                <td>
                    {{row.detail.totalGst}}
                </td>
                <td>
                    {{ row.detail.totalAmount }}
                </td>
                <td>
                    <button class="button is-info" @click="editEntry(row._id)">Edit</button>
                </td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import Datastore from "nedb";

export default {
  name: "editSales",
  components: {
  },
  data() {
    return {
      rows: [],
      db: {},
      showModifyModel:false,
      type:"SALES",
      row:{},
      searchSelectType:0,
      party: [],
      selectedParty: "cash",
    };
  },
  created() {
    this.db.sales = new Datastore({ filename: "sales", autoload: true });
    this.db.sales.find({}, (err, docs) => {
      if (err) {
        alert("Database Error", "Stock Manager");
      } else {
        docs.forEach(d => {
          this.rows.push(d);
        });
      }
    });
    this.db.party = new Datastore({ filename: "party", autoload: true });
    this.db.party.find({}, (err, docs) => {
      if (err !== null) {
        alert("Error");
        console.log(err);
      } else {
        docs.forEach(d => {
           this.party.push(d);
        });
      }

    });
    
  },
  mounted(){
      this.$electron.ipcRenderer.on('reload',()=>{
          console.log("reload called");
          this.reloadRows();
      })
  },
  watch: {
    selectedParty:function(){
      name  = this.selectedParty.name;
      let newRows = [];
      let patt = new RegExp("^"+name+"");
      for(let i = 0;i< this.rows.length;i++){
          if(this.rows[i].detail.party.match(patt)!=null){
              newRows.push(this.rows[i]);      
          }
      }
       this.rows = newRows.splice();
    }
  },
  computed:{
    filterList:function(){
      
    }
    // end of computes
  },
  methods:{
      searchSelectChanged(e){
          this.searchSelectType = e.target.selectedIndex;
          console.log(this.searchSelectType)
      },
      editEntry(id){
          this.$electron.ipcRenderer.send("edit",id);
      },

      reloadRows(){
          this.rows = [];
          this.db.sales = new Datastore({ filename: "sales", autoload: true });
          this.db.sales.find({}, (err, docs) => {
            if (err) {
                alert("Database Error", "Stock Manager");
            } else {
                docs.forEach(d => {
                this.rows.push(d);
                });
            }
          });
      },
  }
};
</script>

<style scoped>

</style>
