<template>
    <div class="container">
        <div class="row mt-5">
          <div class="col-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title">Users Table</h3>

                <div class="card-tools">
                    <button class="btn btn-success" data-toggle="modal" data-target="#addNew">
                      Add New User<i class="fa fa-user-plus "></i>
                    </button>
                </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0">
                <table class="table table-hover">
                  <tbody><tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Type</th>
                    <th>Registered At</th>
                    <th>Modify</th>
                  </tr>
                  <tr v-for="user in users" :key="user.id">
                    <td>{{user.id}}</td>
                    <td>{{user.name}}</td>
                    <td>{{user.email}}</td>
                    <td>{{user.type | upText}}</td>
                    <td>{{user.created_at | myDate}}</td>
                    <td>
                      <a href="#">
                          <i class="fa fa-edit orange"></i>
                      </a>
                      &nbsp;&nbsp;&nbsp;
                      <a href="#">
                          <i class="fa fa-trash red"></i>
                      </a>
                    </td>
                  </tr>
                </tbody></table>
              </div>
              <!-- /.card-body -->
            </div>
            <!-- /.card -->
          </div>
    </div>
<!-- Modal -->
        <!-- Modal -->
          <div class="modal fade" id="addNew" tabindex="-1" role="dialog" aria-labelledby="addNewLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered"  role="document">
              <div class="modal-content">
                <div class="modal-header">
                  <h5 class="modal-title" id="addNewLabel">Add User</h5>
                  <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                  </button>
                </div>
               <form @submit.prevent="createUser">
                <div class="modal-body">
                     <!--starts here-->
                    <div class="form-group">
                         <input v-model="form.name" type="text" name="name" placeholder="Name" 
                         class="form-control" :class="{'is-invalid': form.errors.has('name')}">
                        <has-error :form="form" field="name"></has-error>
                     </div>
                     <div class="form-group">
                         <input v-model="form.email" type="email" name="email" placeholder="Email: example@example.com" 
                         class="form-control" :class="{'is-invalid': form.errors.has('email')}">
                        <has-error :form="form" field="email"></has-error>
                     </div>
                     
                     <div class="form-group">
                         <select v-model="form.type" type="text" name="type" placeholder="User type" 
                         class="form-control" :class="{'is-invalid': form.errors.has('type')}">
                              <option value="">Select user role</option>
                              <option value="admin">Admin</option>
                              <option value="user">Standard User</option>
                              <option value="author">Author</option>
                         </select>
                        <has-error :form="form" field="type"></has-error>
                     </div>
                     <div class="form-group">
                         <textarea v-model="form.bio" type="text" name="bio" placeholder="short bio for the user(optional)" 
                         class="form-control" :class="{'is-invalid': form.errors.has('bio')}"></textarea>
                        <has-error :form="form" field="bio"></has-error>
                     </div>
                     <div class="form-group">
                         <input v-model="form.password" type="password" name="password" placeholder="password" 
                         class="form-control" :class="{'is-invalid': form.errors.has('password')}">
                        <has-error :form="form" field="password"></has-error>
                     </div>
                     
                     <!-- ends here-->
                </div>
                <div class="modal-footer">
                  <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                  <button type="submit" class="btn btn-primary">Create</button>
                </div>
                </form>
              </div>
            </div>
          </div>
    </div>
</template>

<script>
    export default {
      data(){
        return{
          users : {},
            form: new Form({
              name : '',
              email : '',
              password :'',
              type : '',
              bio : '',
              photo : ''
            })
        }
      },

      methods: {
        loadUsers(){
           axios.get("api/user").then(({data})=>(this.users = data.data));
        },
              createUser(){
                this.form.post('api/user');
              }
      },
        created() {
            this.loadUsers();
        }
    }
</script>
