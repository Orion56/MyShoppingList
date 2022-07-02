<template>
    <section id="shopping">
        <h1><i class="bi bi-cart-check-fill"></i> {{ compName }}</h1>
        <h5>Start adding new items</h5>
        <form v-show="showAddForm" @submit.prevent="">
            <input @keyup.enter="saveNew" type="text" name="" id="" placeholder="Type here [hit Enter] to add a new item"
                v-model.trim="newItem">
        </form>
        <div class="controls">
            <label for="itemPriority" class="grab">
                <input v-model="priority" type="checkbox" id="itemPriority">
                High Priority!
            </label>
            <button @click="saveNew" type="button" :disabled="newItem.length < 3">
                Save Item
            </button>
            <button @click="toggleSortByP" type="button" :disabled="priorityFirst.length == 0"
                :class="{ pSorted: sortByP }">
                Sort by Priority
            </button>
        </div>
        <p class="emptyList" v-if="items.length === 0">Your Shopping list is Empty..</p>
        <div v-show="!sortByP" v-for="item in items" :key="item.id">
            <li :class="{ imp: item.priority, done: item.done }" @click="toggleDone(item)">
                {{ items.indexOf(item) + 1 }}- {{ item.value }}
            </li>
        </div>
        <div v-show="sortByP" v-for="item in priorityFirst" :key="item.id">
            <li :class="{ imp: item.priority, done: item.done }" @click="toggleDone(item)">
                {{ priorityFirst.indexOf(item) + 1 }}- {{ item.value }}
            </li>
        </div>
    </section>
</template>

<script>
export default {
    data() {
        return {
            compName: "Shopping List",
            items: [],
            newItem: "",
            priority: false,
            done: false,
            sortByP: false,
            showAddForm: true
        }
    },
    methods: {
        saveNew() {
            if (this.newItem != '' && this.newItem.length >= 3) this.items.push({ id: this.items.length + 1, value: this.newItem, priority: this.priority, done: this.done })
            this.newItem = ''
            this.priority = false
        },
        toggleForm(e) {
            if (window.scrollY > 100) this.showAddForm = !this.showAddForm
            //this.showAddForm=!this.showAddForm
        },
        toggleDone(item) {
            item.done = !item.done
        },
        toggleSortByP() {
            this.sortByP = !this.sortByP
        }
    },
    computed: {
        priorityFirst() {
            let imp = this.items.filter(item => item.priority == true && item.done == false)
            let norm = this.items.filter(item => item.priority == false && item.done == false)
            let done = this.items.filter(item => item.done == true)
            //let pF=imp.push(...norm) 
            return imp.concat(norm, done)
        },
        // itemDynamicID(arr,item){
        //     return arr.indexOf(item)
        // }

    },
    created() {
        //window.addEventListener("scroll", this.toggleForm)
    }
}
</script>

<style>
#shopping {
    width: 70%;
    padding: 10px;
    margin: 5px auto;
    border: solid red 1px;
    text-align: center;
    min-height: 60vh;
}

#shopping h5 {
    margin-bottom: 5px;
}

#shopping form {
    display: flex;
    flex-flow: row nowrap;
    justify-content: center;
}

/* #shopping form input::after {
    font-family: 'bsIcons';
    content: '&#xF338';
    right: 10px;
    font-size: x-large;
    color: crimson;
} */
#shopping li {
    list-style: none;
    text-align: left;
    border-radius: 15px;
    padding: 8px;
    transition: background-color ease-in-out 0.5s;
    /* counter-reset: list; */
}

/* #shopping li:before{
    content: counter(list);margin-right: 5px;
    position: absolute;
    right: 100%;
} */
#shopping li:hover {
    background-color: rgba(172, 255, 47, 0.259);
    cursor: grab;
}

#shopping input {
    padding: 8px;
    border-style: none;
    border-radius: 12px;
}

#shopping button {
    margin: 5px;
    padding: 8px;
    border-style: none;
    border-radius: 12px;
}

#shopping button:disabled {
    cursor: not-allowed;
}

#shopping input[type=text] {
    width: 70%;
    padding: 10px;
    /*  text-align: center; */
}

#shopping input[type=text]:focus,
#shopping .pSorted,
#shopping div.controls button:enabled:hover {
    box-shadow: inset 0px 0px 15px var(--colorAccent1);
}

#shopping .controls {
    margin: 5px;
}

#shopping .emptyList {
    margin-top: 3em;
}

#shopping .grab {
    cursor: grab;
}

/* toggled classes */
.hide {
    display: none;
}

.imp {
    color: red;
    font-weight: bold;
}

.done {
    text-decoration: line-through;
    color: gray;
}
</style>