<template>
    <div class="select-search">
        <div class="select-search__selected" @click="toggleOptions">
            {{ selectedFruit.text || placeholder }}
        </div>
        <div class="select-search__options" v-if="showOptions">
            <input type="text" v-model="searchTerm" class="select-input__search" @input="searchFruit"
                placeholder="This is a search input" />
            <div class="select-search__option" v-for="fruit in filteredFruits" :key="fruit.value"
                @click="selectFruit(fruit)">
                {{ fruit.text }}
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            selectedFruit: {},
            allFruits: [],
            filteredFruits: [],
            searchTerm: '',
            showOptions: false,
            placeholder: 'Select an item',
            valueField: 'value',
            textField: 'text',
        };
    },
    mounted() {
        axios.get('http://127.0.0.1:8888/api/fruits')
            .then(response => {
                this.allFruits = response.data.data.fruits.map(fruit => ({ value: fruit, text: fruit }));
                this.filteredFruits = this.allFruits;
            })
            .catch(error => {
                console.error(error);
            });
    },
    methods: {
        toggleOptions() {
            this.showOptions = !this.showOptions;
        },
        selectFruit(fruit) {
            this.selectedFruit = fruit;
            this.showOptions = false;
        },
        searchFruit() {
            this.filteredFruits = this.allFruits.filter(fruit =>
                fruit.text.toLowerCase().includes(this.searchTerm.toLowerCase())
            );
        },
    },
};
</script>

<style>
.select-search {
    position: relative;
    display: inline-block;
}

.select-search__selected {
    background-color: white;
    padding: 16px;
    cursor: pointer;
    color: #A0AEC0;
    box-shadow: 0px 10px 15px rgba(35, 78, 82, 0.1);
    border-radius: 8px;
    text-align: initial;
    width: 260px;
    height: 56px;
}

.select-search__selected:first-letter {
    text-transform: uppercase;
}

.select-search__options {
    background-color: white;
    position: absolute;
    left: 0;
    width: 280px;
    max-height: 284px;
    overflow-y: scroll;
    border-radius: 8px;
    box-shadow: 0px 10px 15px rgba(35, 78, 82, 0.1);
    padding: 16px;
}

.select-input__search {
    width: 230px;
    border: none;
    background-color: white;
    font-size: 16px;
    color: #A0AEC0 ;
}

.select-search__option {
    padding: 8px;
    cursor: pointer;
    text-align: initial;
    height: fit-content;
    color: #1A202C;
    font-weight: 600;
    font-size: 14px;
    line-height: 24px;
}

.select-search__option:first-letter {
    text-transform: uppercase;
}

.select-search__option:hover {
    color: #4299E1;
}


</style>