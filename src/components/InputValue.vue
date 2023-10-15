<template>
    <div>
        <input v-model="this.point.x">
        <input v-model="this.point.y">
        <input v-model="this.point.marker">
        <button @click="AddPoint">submit</button>
    </div>
    <ul>
        <li v-for="item in this.pointarray" :key="item.id">
            {{ item }}
            <button @click="DeleteRecord(item)">X</button>
        </li>
    </ul>
</template>

<script>
import { MongoClient } from 'mongodb'

export default {
    data() {
        return {
            pointarray: [],
            point: {
                id: 1,
                x: '',
                y: '',
                marker: ''
            }
        }
    },
    methods: {
        AddPoint() {
            let tmp = {
                id: '',
                x: '',
                y: '',
                marker: ''
            }
            tmp.id = this.point.id;
            tmp.x = this.point.x;
            tmp.y = this.point.y;
            tmp.marker = this.point.marker;
            this.pointarray.push(tmp);
            this.point.id++;
            this.point.marker = '';
            this.point.x = '';
            this.point.y = '';
        },
        DeleteRecord(item) {
            this.pointarray = this.pointarray.filter((t) => t !== item)
        },
        mongo() {
            const connectionString = 'mongodb://localhost:27017';
            // const client = new MongoClient(connectionString, { useNewUrlParser: true });
            // client.connect();
            console.log('Connected to MongoDB successfully!');
        }
    }

}
</script>