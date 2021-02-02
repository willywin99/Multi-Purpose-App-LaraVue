<style>
    .widget-user-header{
        background-position: center center;
        background-size: cover;
        /* height: 500px; */
    }
</style>

<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12 mt-3">
                <div class="card card-widget widget-user" style="">
                  <!-- Add the bg color to the header using any of the bg-* classes -->
                  <div class="widget-user-header text-white" style="background-image:url('./img/user-cover.jpg'); height: 300px;">
                    <h3 class="widget-user-username text-right">{{ this.form.name }}</h3>
                    <h5 class="widget-user-desc text-right">Web Designer</h5>
                  </div>
                  <div class="widget-user-image">
                    <img class="img-circle" :src="getProfilePhoto()" alt="User Avatar">
                  </div>
                  <div class="card-footer">
                    <div class="row">
                      <div class="col-sm-4 border-right">
                        <div class="description-block">
                          <h5 class="description-header">3,200</h5>
                          <span class="description-text">SALES</span>
                        </div>
                        <!-- /.description-block -->
                      </div>
                      <!-- /.col -->
                      <div class="col-sm-4 border-right">
                        <div class="description-block">
                          <h5 class="description-header">13,000</h5>
                          <span class="description-text">FOLLOWERS</span>
                        </div>
                        <!-- /.description-block -->
                      </div>
                      <!-- /.col -->
                      <div class="col-sm-4">
                        <div class="description-block">
                          <h5 class="description-header">35</h5>
                          <span class="description-text">PRODUCTS</span>
                        </div>
                        <!-- /.description-block -->
                      </div>
                      <!-- /.col -->
                    </div>
                    <!-- /.row -->
                  </div>
                </div>
            </div>
        </div>

        <div class="card">
              <div class="card-header p-2">
                <ul class="nav nav-pills">
                  <li class="nav-item"><a class="nav-link" href="#activity" data-toggle="tab">Activity</a></li>
                  <li class="nav-item"><a class="nav-link active" href="#settings" data-toggle="tab">Settings</a></li>
                </ul>
              </div><!-- /.card-header -->
              <div class="card-body">
                <div class="tab-content">
                  <div class="tab-pane" id="activity">
                    <center> <h3> Display User Activity </h3> </center>
                  </div>

                  <div class="tab-pane active" id="settings">
                    <form class="form-horizontal">
                      <div class="form-group row">
                        <label for="inputName" class="col-sm-2 col-form-label">Name</label>
                        <div class="col-sm-10">
                          <input type="text" v-model="form.name" class="form-control" id="inputName" placeholder="Name" :class="{ 'is-invalid':form.errors.has('name') }">
                          <has-error :form="form" field="name"></has-error>
                        </div>
                      </div>
                      <div class="form-group row">
                        <label for="inputEmail" class="col-sm-2 col-form-label">Email</label>
                        <div class="col-sm-10">
                          <input type="email" v-model="form.email" class="form-control" id="inputEmail" placeholder="Email" :class="{ 'is-invalid':form.errors.has('email') }">
                          <has-error :form="form" field="email"></has-error>
                        </div>
                      </div>
                      <div class="form-group row">
                        <label for="inputExperience" class="col-sm-2 col-form-label">Experience</label>
                        <div class="col-sm-10">
                          <textarea
                            class="form-control"
                            id="inputExperience"
                            placeholder="Experience"
                            :class="{ 'is-invalid':form.errors.has('bio') }"
                          >
                          </textarea>
                          <has-error :form="form" field="bio"></has-error>
                        </div>
                      </div>
                      <div class="form-group row">
                        <label for="inputPhoto" class="col-sm-2 col-form-label">Profile Photo</label>
                        <div class="col-sm-10">
                          <input type="file" class="form-control" name="photo" @change="updateProfile" id="inputPhoto" placeholder="Photo">
                        </div>
                      </div>
                      <div class="form-group row">
                        <label for="inputPassword" class="col-sm-2 col-form-label">Password</label>
                        <div class="col-sm-10">
                          <input
                            type="password"
                            v-model="form.password"
                            class="form-control"
                            id="inputPassword"
                            placeholder="Password... (leave empty if not changing)"
                            :class="{ 'is-invalid':form.errors.has('password') }"
                          >
                          <has-error :form="form" field="password"></has-error>
                        </div>
                      </div>
                      <div class="form-group row">
                        <div class="offset-sm-2 col-sm-10">
                          <button @click.prevent="updateInfo" type="submit" class="btn btn-success">Update</button>
                        </div>
                      </div>
                    </form>
                  </div>
                  <!-- /.tab-pane -->
                </div>
                <!-- /.tab-content -->
              </div><!-- /.card-body -->
            </div>
    </div>
</template>

<script>
    import Form from 'vform'
    export default {
        data() {
            return {
                userPhoto:"",
                form: new Form({
                    id: '',
                    name: '',
                    email: '',
                    password: '',
                    type: '',
                    bio: '',
                    photo: ''
                })
            }
        },
        mounted() {
            console.log('Component mounted.')
        },
        methods: {
            getProfilePhoto() {

                let photo = (this.form.photo.length > 200) ? this.form.photo : "img/profile/"+ this.form.photo ;
                return photo;

                // return "img/profile/"+ this.form.photo;
            },
            updateInfo() {
                this.$Progress.start();
                if(this.form.password == '') {
                    this.form.password = undefined;
                }
                this.form.put('api/profile')
                    .then( () => {
                        Fire.$emit('AfterCreate');
                        this.$Progress.finish();
                    })
                    .catch( () => {
                        this.$Progress.fail();
                    })

                // this.$Progress.start()
                // this.form.put('api/profile')
                //     .then(() => {
                //         this.$Progress.finish()
                //         // Toaster form Sweet Alert
                //         toast({
                //             type:'success',
                //             title:'Profile updated!'
                //         })
                //         Fire.$emit('AfterUpdate');
                //     })
                //     .catch(() => {
                //         this.$Progress.fail()

                //         // Toaster from Sweet Alert
                //         toast({
                //             type:'error',
                //             title:'Something went wrong!'
                //         })
                //     })
            },
            updateProfile(e) {
                // console.log('uploading');
                let file = e.target.files[0];
                console.log(file);
                let reader = new FileReader();
                if(file['size'] < 2111775) {
                    reader.onloadend = (file) => {
                        // console.log('RESULT', reader.result)
                        this.form.photo = reader.result;
                    }
                    reader.readAsDataURL(file);
                } else {
                    swal.fire({
                        type: 'error',
                        title: 'Oops...',
                        text: 'You are uploading a large file',
                        icon: 'error',
                    })
                }
                // if(this.form.password == "") {
                //     this.form.password = undefined;
                // }
            }
        },
        created() {
            axios.get("api/profile")
                .then(({ data }) => (this.form.fill(data)));

            // this.$Progress.start()
            // axios.get('api/profile')
            //     .then(({ data }) => {
            //         this.userPhoto = "img/profile/" + data.photo;
            //         Fire.$on('AfterUpdate', () => {
            //             axios.get('api/profile')
            //                 .then((data) => {
            //                     let photo = data.data.photo
            //                     this.userPhoto = "img/profile/" + photo;
            //                 })
            //         })
            //         this.form.reset();
            //         this.form.fill(data);
            //         this.$Progress.finish();
            //     })
            //     .catch(() => {
            //         this.$Progress.fail();
            //     })
        }
    }
</script>
