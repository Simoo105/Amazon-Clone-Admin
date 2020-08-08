<template>
    <main>
        <div class= "container- fluid">
            <div class="row">
                <div class="col-sm-3"></div>
                <div class="col-sm-6">
                    <div class="a-section">
                        <div class="a-spacing-top-medium"></div>
                        <h2 style="text-align: center">Add new Owner</h2>
                        <form  enctype="multipart/form-data">
                            <!-- Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >Name</label>
                                <input type="text" class="a-input-text" style ="width: 100%" v-model="name">
                            </div>
                            <!-- Description Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >About</label>
                                <textarea placeholder="Provide details about the owner" name="" style="width: 100%" v-model="about"></textarea>
                            </div>
                            <!-- Photo Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >Add Photo</label>
                                <div class="a-row a-spacing-top-medium">
                                    <label class="choosefile-button">
                                        <i class="fal fa-plus"></i>
                                        <input type="file" @change="onFileSelected">
                                        <p style="margin-top: -70px">{{fileName}}</p>
                                    </label>
                                </div>
                            </div>
                            <hr>
                            <!-- Button -->
                            <div class="a-spacing-top-large">
                                <span class="a-button-register">
                                    <span class="a-button-inner">
                                        <span class="a-button-text" @click="onAddOwner">Add Owner</span>
                                    </span>
                                </span>
                            </div>
                        </form>
                        <div class="a-spacing-top-medium"></div>
                        <ul class="list-group-item">
                            <li v-for="owner in owners" :key="owner._id">{{owner.name}}</li>
                        </ul>
                    </div>
                </div>
                <div class="col-sm-3"></div>
            </div>
        </div>
    </main>
</template>

<script>
    export default {
        data() {
            return {
                name: '',
                about: '',
                selectedFile:null,
                fileName: ''
            }
        },
        async asyncData({$axios}) {
            try {
                let resp = await $axios.$get('http://localhost:3000/api/owners');
                return {
                    owners: resp.owners
                }
            } catch (err) {
                console.log(err);
            }
        },
        methods: {
            async onAddOwner() {
                try {
                    let data = new FormData();
                    data.append("name", this.name);
                    data.append("about", this.about);
                    data.append("photo", this.selectedFile, this.selectedFile.name);
                    let resp = await this.$axios.$post('http://localhost:3000/api/owners', data)
                    if (resp.success) {
                        this.owners.push({
                            name: this.name,
                            about: this.about
                        });
                    }
                    console.log(this.owners)
                }
                catch (error) {
                    console.log(error);
                }
            },
            onFileSelected(event) {
                this.selectedFile = event.target.files[0];
                console.log(this.selectedFile);
                this.fileName = event.target.files[0].name;
            } 
        }
    }
</script>