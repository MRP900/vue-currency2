<template>
    <div class="form-group">
        <form id="currency-form" @submit="convert">
        <!-- <form id="currency-form" v-on:change="convert"> -->
                <div id="fields">
                    <input id="currency-in" type="text" v-model="money" placeholder="Amount to Convert"><br>
                        <div id="selectors">
                            <select id="dropDown1" v-model="convertFrom" v-on:change="convert">
                                <option value="Convert From" convertFrom>Convert From</option>
                                <option v-for="rate in rates" v-bind:key="rate.id">
                                    {{ rate.abb }} - {{rate.name}}
                                </option> 
                            </select>
                            <br>
                            <select id="dropDown2" v-model="convertTo" v-on:change="convert">
                                <option value="Convert To" convertTo>Convert To</option>
                                <option v-for="(item, index) in rates" v-bind:key=index+1>
                                    {{ item.abb }} - {{ item.name }}
                                </option> 
                            </select>
                        </div>
                </div>
                <div id="buttons" class="">
                    <input type ="submit" value="Convert" class="btn btn-primary">
                    <input type ="reset" @click="clearOut" value="Reset" class="btn btn-danger">
                </div>
        </form>
    </div>
</template>

<script>

export default {
    name: "Convert",
    props: ["rates"],
    data: ()=> {
        return {
            money : "",
            convertFrom: "Convert From",
            convertTo : "Convert To"
        }
    },
    methods: {
        convert(e) {
            e.preventDefault();
            if (!isNaN(this.money) & (this.money.length > 0) & (this.convertTo != "Convert To") & (this.convertFrom != "Convert From")) {
                this.$emit('new-convert', this.money, this.convertFrom, this.convertTo);
            }
        },
        clearOut() {
            this.money = "";
            this.convertTo = "Convert to";
            this.convertFrom = "Convert From";
            this.$emit('clear-out');
        }
    } 
}
</script>

<style scoped>
    legend {
        font-style: italic;
        font-weight: bold;
        text-align: left;
    }
    #currency-form {
        width: 60%;
        margin: 1em auto;
    }

    #fields input, input {
        margin: .25em 1em;
    }
    #buttons {
        margin: 0 auto;
        width: 75%;
    }
    #buttons input {
        margin-right: .4em;
    }
    #selectors {
        width: 100%;
        margin: .25em auto;
    }
    #dropDown1, #dropDown2 {
        margin: .4em auto;
    }
    select {
        width: 95%;
    }

@media only screen and (max-width: 500px) {
    #dropDown1, #dropDown2 {
        margin: .4em 0;
    }
    select {
        width: 75%;
    }
    #currency-form {
        width: 90%;
        margin: 1em auto;
        /* display: flex; */
        
    }

    /* #fields input, input {
        margin: .25em 1em;
    } */
}

    @media only screen and (max-width: 500px) {
    #dropDown1, #dropDown2 {
        margin: .4em 0;
    }
    select {
        width: 90%;
    }
    #currency-form {
        width: 100%;
        margin: 1em auto;
        /* display: flex; */
        
    }

    #fields input, input {
        margin: .25em 0;
    }
}

 @media only screen and (max-width: 390px) {
    #dropDown1, #dropDown2 {
        margin: .4em 0;
    }
    select {
        width: 100%;
    }
    #currency-form {
        width: 100%;
        margin: 1em auto;
        /* display: flex; */
        
    }

    #fields input {
        margin: .25em 0;
    }
    #currency-in {
        width:100%;
    }
}
</style>