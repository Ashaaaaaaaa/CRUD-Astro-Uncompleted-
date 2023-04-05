<template>
    <div class="content" style="top: 50px">
        <slot name="create"></slot>

        <div class="bgcreate mt-5 align-item-center">
                <button type="button" class="crtbtn ms-3 " style="margin-top:13px; font-weight:600;"  data-bs-toggle="modal" data-bs-target="#exampleModal">Create</button>
        </div>
        <hr>
        <!-- @if (session('success'))
        <div class="alert alert-success">
                {{session('success')}}
        </div>
        @endif -->

        <table class="table table-dark table-striped">
            <thead>
                <tr>
                <th scope="col">npm</th>
                <th scope="col">Nama Mahasiswa</th>
                <th scope="col">Fakultas</th>
                <th scope="col">Prodi</th>
                <th scope="col">Edit</th>
                </tr>
            </thead>
            <tbody>

                <tr v-for="(mahasiswa, index) in this.mahasiswas" :key="index" >
                    <th scope="row">{{ mahasiswa.npm }}</th>
                    <td>{{ mahasiswa.name_mahasiswa }}</td>
                    <td>{{ mahasiswa.fakultas }}</td>
                    <td>{{ mahasiswa.prodi }}</td>
                <td>
                    <button class="bg-transparent" style="border: none" @click="deleteMahasiswa(mahasiswa.id)"><a href="#" style="color: rgb(221, 221, 221)"><i class="fa-solid fa-trash"></i></a></button>
                    <button class="bg-transparent ms-2" style="border: none"  data-bs-toggle="modal" :data-bs-target="'#edit' + mahasiswa.id"><i class="fa-solid fa-pen-to-square " style="color: rgb(235, 235, 235)"></i></button>
                    <button class="bg-transparent ms-2" style="border: none"><a href=""><i class="fa-regular fa-eye" style="color: rgb(235, 235, 235)"></i></a></button>
                </td>

                <div
                    class="modal fade"
                    :id="'edit' + mahasiswa.id"
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
                            onclick="window.location.reload();"
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
                                :placeholder="mahasiswa.npm"
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
                                :placeholder="mahasiswa.name_mahasiswa"
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
                                :placeholder="mahasiswa.fakultas"
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
                                :placeholder="mahasiswa.prodi"
                                name="prodi"
                                />
                            </div>
                            </div>

                            <div class="modal-footer">
                            <div class="d-grid gap-2 col-6 mx-auto">
                                <button class="btn btn-outline-success" type="button" @click="updateMahasiswa(mahasiswa.id)">
                                Update
                                </button>
                                <button
                                class="btn btn-outline-danger"
                                type="button"
                                data-bs-dismiss="modal"
                                onclick="window.location.reload();"
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
                </tr>
                
            </tbody>
        </table>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'mahasiswas',
    data(){
        return {
            mahasiswas: [],
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
    mounted(){
        this.getMahasiswas();
        // console.log('i am here')
        this.mahasiswaId = this.model.mahasiswa.id;
    },
    methods: {
        getMahasiswas(){

            axios.get('http://127.0.0.1:8000/api/mahasiswa').then(res => {
                this.mahasiswas = res.data.mahasiswa
                console.log(res)
            })
        },

        deleteMahasiswa(mahasiswaId) {

            console.log(mahasiswaId)
            if(confirm('Are you sure, you want to delete this data?') ) {

                // console.log(mahasiswaId)
                axios.delete(`http://127.0.0.1:8000/api/mahasiswa/destroy/${mahasiswaId}`)
                    .then(res => {

                        alert(res.data.message);
                        this.getMahasiswa();

                        window.location.reload();

                    })
                    .catch(function (error) {

                    if (error.response) {
                        if(error.response.status == 404) {

                            alert(error.response.data.message);
                        }
                    }
                    });
            }
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
    },
}
</script>