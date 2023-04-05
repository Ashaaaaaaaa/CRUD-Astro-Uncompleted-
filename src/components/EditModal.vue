<template>
  <div
    class="modal fade"
    :id="'edit' + model.mahasiswa.id"
    tabindex="-1"
    aria-labelledby="edit"
    aria-hidden="true"
  >
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content text-bg-dark">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="edit">Edit Computer Data</h1>
          <button
            type="button"
            class="btn-close btn-close-white"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <form method="POST" action="">
            <div class="input-group">
              <div class="form-outline mb-4">
                <label class="form-label lbel" for="form2Example11">Npm</label>
                <input
                  type="text"
                  id="form2Example11"
                  style="background-color: rgb(24, 27, 30)"
                  class="form-control text-white"
                  v-model="model.mahasiswa.npm"
                  name="npm"
                />
              </div>

              <div class="form-outline ms-3 mb-4">
                <label class="form-label lbel" for="form2Example11"
                  >Nama Mahasiswa</label
                >
                <input
                  type="text"
                  id="form2Example11"
                  style="background-color: rgb(24, 27, 30)"
                  class="form-control text-white"
                  v-model="model.mahasiswa.name_mahasiswa"
                  name="name_mahasiswa"
                />
              </div>

              <div class="form-outline mb-4">
                <label class="form-label lbel" for="form2Example11"
                  >Fakultas</label
                >
                <input
                  type="text"
                  id="form2Example11"
                  style="background-color: rgb(24, 27, 30)"
                  class="form-control text-white"
                  v-model="model.mahasiswa.fakultas"
                  name="fakultas"
                />
              </div>

              <div class="form-outline ms-3 mb-4">
                <label class="form-label lbel" for="form2Example11"
                  >Prodi</label
                >
                <input
                  type="text"
                  id="form2Example11"
                  style="background-color: rgb(24, 27, 30)"
                  class="form-control text-white"
                  v-model="model.mahasiswa.prodi"
                  name="prodi"
                />
              </div>
            </div>

            <div class="modal-footer">
              <div class="d-grid gap-2 col-6 mx-auto">
                <button class="btn btn-outline-success" type="button" @click="updateMahasiswa(model.mahasiswa.id)">
                  Update
                </button>
                <button
                  class="btn btn-outline-danger"
                  type="button"
                  data-bs-dismiss="modal"
                >
                  Cancel
                </button>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'mahasiswaEdit',
    data() {
        return {
            mahasiswaId: '',
            errorList: '',
            model: {
                mahasiswa: {
                    npm: '',
                    name_mahasiswa: '',
                    fakultas: '',
                    prodi: ''
                }
            }
        }
    },
    mounted() {

      this.mahasiswaId = this.model.mahasiswa.id;
      this.getMahasiswaData(this.model.mahasiswa.id);
    },
    methods: {

        getMahasiswaData(mahasiswaId) {

            axios.get(`http://127.0.0.1:8000/api/mahasiswa/${mahasiswaId}/edit`)
                .then(res => {
                    console.log(res.data.mahasiswa);

                    this.model.mahasiswa = res.data.mahasiswa
                })
                .catch(function (error) {

                  if (error.response) {
                      if(error.response.status == 404) {

                          alert(error.response.data.message);
                      }
                  }
                  });
        },


        updateMahasiswa(mahasiswaId) {

            var $this = this;
            axios.put(`http://127.0.0.1:8000/api/mahasiswa/${mahasiswaId}/edit`, this.model.mahasiswa)
                .then(res => {
                    console.log(res.data)
                    alert(res.data.message);

                    this.errorList = '';

                    window.location.reload();
                })
                .catch(function (error) {

                    if (error.response) {
                        if(error.response.status == 422) {

                            $this.errorList = error.response.data.errors;
                        }
                        if(error.response.status == 404) {

                            alert(error.response.data.message);
                        }
                    } else if (error.request) {
                    console.log(error.request);
                    } else {
                    console.log('Error', error.message);
                    }
                })
        }
    }
}
</script>