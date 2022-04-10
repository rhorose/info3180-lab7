<template>
    
    <div> 
        <h2> Upload Form </h2>

        <div v-if="success" class="alert alert-success" role="alert">
           <p> File Upload Successful </p>


        <div v-if="errors.length" class="alert alert-danger" role="alert">
            <ul>
                <li v-for="error in errors" :key="errors.indexOf(error)">{{ error }}</li>
            </ul>
        </div>

        
        </div>
        

        <form @submit.prevent="uploadPhoto" method="POST" enctype="multipart/form-data" id="uploadForm">
            <div class="form-group">
                <label for="description"> Description </label><p>
                <textarea name="description" class="form-control"></textarea><p>
                
                <label for="photo"> Photo Upload </label><br>
                <input type="file" name="photo" class="form-control" id="photo"><p>
            </div>
            <button type="submit" class="btn btn-primary mb-2">Submit</button>
        </form>
    </div>
</template>
    
<script>
    export default {
        data() {
            return {
                errors: [],
                success: false,
                 csrf_token: '' 
            };
        },
        created() {
            this.getCsrfToken();
        },
        methods: {
            uploadPhoto(){
                let uploadForm = document.getElementById('uploadForm');
                let formdata = new Form_Data(uploadForm);
                let self = this

                fetch("/api/upload", {
                    method: 'POST',
                    body: ,
                    headers: {
                        'X-CSRFToken': this.csrf_token
                    }
                })
                .then(function (response) {
                    return response.json();
                    })
                .then(function (data) {
                    // display a success message
                    console.log(data);
                    if ('errors' in data){
                        self.errors = [...data.errors]
                        self.success = false
                    } 
                    else {
                        self.errors = []
                        self.success = true
                    }
                })
                    .catch(function (error) {
                        console.log(error);
                        });
            },
            getCsrfToken() {
                let self = this;
                fetch('/api/csrf-token')
                    .then((response) => response.json())
                    .then((data) => {
                        console.log(data);
                        self.csrf_token = data.csrf_token;
                })
            }
        }
    }

</script>