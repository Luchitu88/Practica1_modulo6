<template>
    <div class="about">
        <h5 class="mb-5 mt-5">Mis Contactos</h5>

        <div>
            <div class="mb-3">
                <div class="input-group">
                    <span class="input-group-text" id="type-filter"><i
                            class="bi bi-filter "></i>  Filtrar por Pais </span>
                    <select class="form-select" @change="toFilter = $event.target.value">
                        <option value="">todos</option>
                        <option v-for="pais in paises" :value="pais" :key="pais">{{pais}}</option>
                    </select>
                </div>
            </div>

            <div class="mb-3">
                <div class="input-group">
                    <span class="input-group-text" id="description-search-text"><i class="bi bi-search"></i> Buscar por Nombre </span>
                    <input type="search" class="form-control" id="nombre-search"
                           @search="this.toSearch = $event.target.value">
                </div>
            </div>
        </div>

        <div class="card">
            <div class="card-body">
                <form @submit.prevent="save()">
                    <table class="table table-striped">
                        <thead>
                        <tr>
                            
                            <th colspan="2">

                                <input type="text" class="form-control" id="nombre" v-model="newLink.nombre">

                            </th>
                            <th>

                                <input type="text" class="form-control" id="correo" v-model="newLink.correo">

                            </th>
                            <th>

                                <input type="text" class="form-control" id="direccion" v-model="newLink.direccion">

                            </th>
                            <th>

                                <input type="text" class="form-control" id="telefono" v-model="newLink.telefono">

                            </th>   
                            <th>

                                <select class="form-select" aria-label="Default select example" v-model="newLink.pais">
                                    <option v-for="pais in paises" :value="pais" :key="pais">{{pais}}</option>
                                </select>

                             </th>                        
                            
                            <th>

                                <input type="text" class="form-control" id="ciudad" v-model="newLink.ciudad">

                            </th>                           
 
                            <th>
                                <button type="submit" class="btn btn-primary">Agregar</button>
                            </th>
                        </tr>
                        <tr>
                            <th scope="col">Id</th>
                            <th scope="col">Nombre</th>
                            <th scope="col">Correo</th>
                            <th scope="col">Direccion</th>
                            <th scope="col">Telefono</th>
                            <th scope="col">Pais</th>
                            <th scope="col">Ciudad</th>
                            <th></th>
                        </tr>
                        </thead>

                        <tbody>
                        <tr v-for="(link, index) in getList()" :key="index">
                            <th scope="row">{{1+index}}</th>
                            <td>{{link.nombre}}</td>
                            <td>{{link.correo}}</td>
                            <td>{{link.direccion}}</td>
                            <td>{{link.telefono}}</td>
                            <td>{{link.pais}}</td>
                            <td>{{link.ciudad}}</td>
                            
                            <td>
                                <button type="button" class="btn btn-info btn-sm" @click="edit(index)"><i
                                        class="bi bi-pen"></i></button>

                                <button type="button" class="btn btn-danger btn-sm" @click="remove(index)"><i
                                        class="bi bi-trash3"></i></button>
                            </td>
                        </tr>
                        </tbody>
                    </table>
                </form>
            </div>
        </div>

        <!-- Modal -->
        <div class="modal fade" id="editModal" tabindex="-1" aria-labelledby="editModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h1 class="modal-title fs-5" id="editModalLabel">Editar</h1>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <form @submit.prevent="saveEdit()">
                        <div class="modal-body" v-if="itemSelected">
                            
                            <div class="mb-3">
                                <label for="updateNombre" class="form-label">Nombre</label>
                                <input type="text" v-model="itemSelected.nombre" class="form-control"
                                       id="updateNombre">
                            </div>
                            <div class="mb-3">
                                <label for="updateCorreo" class="form-label">Correo</label>
                                <input type="text" v-model="itemSelected.correo" class="form-control"
                                       id="updateCorreo">
                            </div>
                            <div class="mb-3">
                                <label for="updateDireccion" class="form-label">Direccion</label>
                                <input type="text" v-model="itemSelected.direccion" class="form-control"
                                       id="updateDireccion">
                            </div>
                            <div class="mb-3">
                                <label for="updateTelefono" class="form-label">Telefono</label>
                                <input type="text" v-model="itemSelected.telefono" class="form-control"
                                       id="updateTelefono">
                            </div>
                            <div class="mb-3">
                                <label for="updatePais" class="form-label">Pais</label>
                                <select class="form-select" v-model="itemSelected.pais" id="updatePais">
                                    <option v-for="pais in paises" :value="pais">{{pais}}</option>
                                </select>
                            </div>
                            <div class="mb-3">
                                <label for="updateCiudad" class="form-label">Ciudad</label>
                                <input type="text" v-model="itemSelected.ciudad" class="form-control"
                                       id="updateCiudad">
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                            <button type="submit" class="btn btn-primary">Guardar cambios</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        data() {
            return {
                paises: ['Bolivia', 'Peru', 'Canada', 'USA', 'Mexico'],
                newLink: {
                    nombre: '',
                    correo: '',
                    direccion: '',
                    telefono:'',
                    pais: '',
                    ciudad: ''
                },
                linksArray: [
                    {
                        
                        nombre: "Alice Johnson",
                        correo: "alice.johnson@example.com",
                        direccion: "123 Maple Street",
                        telefono: "123-456-7890",
                        pais: "USA",
                        ciudad: "New York"
                    },
                    {
                        nombre: "Bob Smith",
                        correo: "bob.smith@example.com",
                        direccion: "456 Oak Avenue",
                        telefono: "87-654-3210",
                        pais: "Canada",
                        ciudad: "Toronto"
                    },
                    {
                        nombre: "Juan Pablo Perez",
                        correo: "juan.perez@gmail.com",
                        direccion: "Sopocachi No. 50",
                        telefono: "78562398",
                        pais: "Bolivia",
                        ciudad: "La Paz"
                    },
                    {
                        nombre: "Juana Rodriguez",
                        correo: "juanita.bonitah@gmail.com",
                        direccion: "Miraflores No. 524",
                        telefono: "78965423",
                        pais: "Canada",
                        ciudad: "Toronto"
                    },
                    {
                        nombre: "Luis Miguel Sanchez",
                        correo: "luismi.sanchez@gmail.com",
                        direccion: "Heroinas  No. 25",
                        telefono: "78941236",
                        pais: "Bolivia",
                        ciudad: "Cochabamba"
                    }
                    
                ],
                itemSelected: null,
                indexSelected: null,
                typeFilter: '',
                toFilter: '',
                toSearch: ''
            }
        },
        methods: {
            save() {
                // Validar que los campos no estén vacíos
                if (this.newLink.pais && this.newLink.nombre && this.newLink.correo && this.newLink.direccion 
                    && this.newLink.telefono && this.newLink.ciudad) {
                    // Agregar un nuevo objeto al array
                    /**
                     * const newObj {type: this.newlink.type}
                     * 
                     * const newObj = {...this.newLink}
                     * */
                    this.linksArray.push({...this.newLink});
                    // Limpiar los campos del formulario
                    this.newLink.nombre = '';
                    this.newLink.correo = '';
                    this.newLink.direccion = '';
                    this.newLink.telefono = '';
                    this.newLink.pais = '';
                    this.newLink.ciudad = '';
                } else {
                    alert("Por favor, completa todos los campos.");
                }
            },
            remove(index) {
                if (confirm("¿Está seguro de eliminar este ítem?")) {
                    this.linksArray.splice(index, 1);
                }
            },
            edit(index) {
                this.itemSelected = {...this.linksArray[index]};
                this.indexSelected = index;
                const editModal = new bootstrap.Modal(document.getElementById('editModal'));
                editModal.show();
            },
            saveEdit() {
                this.linksArray[this.indexSelected] = {...this.itemSelected};

                const modalElement = document.getElementById('editModal');
                const modalInstance = bootstrap.Modal.getInstance(modalElement) || new bootstrap.Modal(modalElement);
                modalInstance.hide();

                this.itemSelected = null;
                this.indexSelected = null;
            },
            getList() {
                let result = this.linksArray.filter((item) => {
                    if (this.toSearch) {
                        return item.nombre.includes(this.toSearch);
                    }
                    return true;
                });
                return result.filter((item) => {
                    if (this.toFilter) {
                        return item.pais === this.toFilter;
                    }
                    return true;
                });
                // return this.linksArray;
            }

        }
    }
</script>

<style>
    .btn {
        margin-right: 3px;
    }
    .card{
        overflow-x: auto;
    }
</style>