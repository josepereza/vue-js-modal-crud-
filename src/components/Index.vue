<template>
    <div>
        <h1>Items</h1>
      <div class="titulo_boton">
    Información adicional
   <button class="btn btn-danger"  v-on:click="muestra_oculta('contenido')">Ocultar</button>
</div>

<div id="contenido">
<p>Aquí va el contenido a mostrar<br>
<br>Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing </p>
</div>




        <!--edit Button trigger modal -->


<!-- Modal -->
<modal name="edit" :resizable="true" :draggable="true"  :adaptive="true" :height="650" @before-open="beforeOpen" >
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="editModalLabel">Modal title</h5>
        
      </div>
      <div class="modal-body">
       <div class="card">
            <div class="card-header">
                <h3>Update Item</h3>
            </div>
            <div class="card-body">
                <form v-on:submit.prevent="updateItem(id,name,price)">
                  <div class="form-group">
                        <label>Item Id:</label>
                        <input type="text" class="form-control" v-model="id"/>
                    </div>
                    <div class="form-group">
                        <label>Item Name:</label>
                        <input type="text" class="form-control" v-model="name"/>
                    </div>
                    <div class="form-group">
                        <label>Item Price:</label>
                        <input type="text" class="form-control" v-model="price"/>
                    </div>
                    <div class="form-group">
                        <input type="submit" class="btn btn-primary" value="Update Item"/>
                    </div>
                </form>
            </div>
        </div>
      </div>
      <div class="modal-footer">
         <div slot="top-right">
      <button @click="$modal.hide('edit')">
        ❌
      </button>
    </div>
      </div>
    </div>
 
</div>
</modal>
<!--2 Button trigger modal -->
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
  Add Item
</button>

<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
       <div class="card">
            <div class="card-header">
                <h3>Add Item</h3>
            </div>
            <div class="card-body">
                <form v-on:submit.prevent="addItem(creaname,creaprice)">
                    <div class="form-group">
                        <label>Item Name:</label>
                        <input type="text" class="form-control" v-model="creaname"/>
                    </div>
                    <div class="form-group">
                        <label>Item Price:</label>
                        <input type="text" class="form-control" v-model="creaprice"/>
                    </div>
                    <div class="form-group">
                        <input type="submit" class="btn btn-primary" value="Add Item"/>
                    </div>
                </form>
            </div>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
        <table class="table table-hover">
            <thead>
            <tr>
                <td>ID</td>
                <td>Item Name</td>
                <td>Item Price</td>
                <td>Actions</td>
            </tr>
            </thead>

            <tbody>
                <tr v-for="item in items" :key="item._id">
                    <td>{{ item._id }}</td>
                    <td>{{ item.name }}</td>
                    <td>{{ item.price }}</td>
                    <td><button type="button" v-on:click="edit(item._id,item.name,item.price)">
                        Edit
                        </button></td>
                    <td><button class="btn btn-danger"  v-on:click="deleteItem(item._id)">Delete</button></td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>

    export default {
        data(){
            return{
                items: [],
                item: {},
                id:'',
                name:'',
                price:null,
                creaname: '',
                creaprice: '',
                item2: {}
            }
        },

        created: function()
        {
            this.fetchItems();
        },

        methods: {
            fetchItems()
            {
              let uri = 'http://173.249.39.9:4000/items';
              this.axios.get(uri).then((response) => {
                  this.items = response.data;
              });
            },
            deleteItem(id)
            {
              let uri = 'http://173.249.39.9:4000/items/delete/'+id;
              
              this.axios.get(uri);
               this.fetchItems();
               this.creaname='';
                this.creaprice='';
            },
            addItem(name,price) {
           let uri = 'http://173.249.39.9:4000/items/add';
           console.log(this.item);
            this.item2.name=name;
            this.item2.price=price;
            this.axios.post(uri, this.item2).then(response => this.fetchItems());
        },
        edit(id,nombre,precio){
            
            this.$modal.show('edit', { n1: nombre,p1: precio,i1:id })
        },

            updateItem(id,name,price)
            {
             this.item._id=id;
             this.item.name=name;
             this.item.price=price;

              let uri = 'http://173.249.39.9:4000/items/update/'+id;
              
                this.axios.post(uri, this.item).then(response => {this.fetchItems();
                this.item._id='';
                this.item.name='';
                this.item.price=null;
                this.creaname='';
                this.creaprice='';
                this.id=null;
                }
                );
                
                },
            
        muestra_oculta(id){
//se obtiene el id
var el = document.getElementById(id); //se define la variable "el" igual a nuestro div
el.style.display = (el.style.display == 'none') ? 'block' : 'none'; //damos un atributo display:none que oculta el div

},
 beforeOpen (event) {
    this.id=event.params.i1
            this.name=event.params.n1
            this.price=event.params.p1
  }
        }
    }
</script>

<style>
#contenido{
  float:left;
  clear:both;
  border:2px solid #e6e6e6;
  margin-top:2px;
  padding:5px;
  width:396px;
  overflow:auto;
  font-family:helvetica;
  font-size:14px;
  text-align: justify;
}
</style>
