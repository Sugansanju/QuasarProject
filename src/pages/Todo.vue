<template>
<q-layout view="lHh Lpr lff" container style="height:100vh" class="shadow-2 rounded-borders ">
    <q-header class="bg-white text-primary">
        <q-toolbar>
            <q-toolbar-title class="text-right">
                <q-btn round dense unelevated="" icon="menu" :ripple="false" class="q-ma-sm bg-grey-3 text-black" @click="drawer = !drawer" />
            </q-toolbar-title>
        </q-toolbar>
    </q-header>
    <q-drawer v-model="drawer" show-if-above :width="250" :breakpoint="800">
        <div style="height: calc(100% - 150px); margin-top: 150px; border-right: 2px solid #f5f5f5">
            <q-list padding class="menu-list text-styl text-grey-6">
                <q-item clickable v-ripple active>
                    <q-item-section avatar>
                        <q-icon name="format_list_bulleted" />
                    </q-item-section>
                    <q-item-section>
                        Inbox
                    </q-item-section>

                    <q-item-section avatar>
                            <div class="q-pa-xs text-center" style="border:2px solid #efefef;border-radius:70px;height:30px;width:30px;">
                                5
                            </div>
                    </q-item-section>
                </q-item>

                <q-item clickable v-ripple>
                    <q-item-section avatar>
                        <q-icon name="thumb_up_off_alt" />
                    </q-item-section>
                    <q-item-section>
                        Done
                    </q-item-section>
                    <q-item-section avatar>
                            <div class="q-pa-xs text-center text-green" style="border:2px solid #efefef;border-radius:70px;height:30px;width:30px;">
                                4
                            </div>
                    </q-item-section>
                </q-item>
                <q-item clickable v-ripple>
                    <q-item-section avatar>
                        <q-icon name="star_outline" />
                    </q-item-section>

                    <q-item-section>
                        Important
                    </q-item-section>
                    <q-item-section avatar>
                            <div class="q-pa-xs text-center text-orange" style="border:2px solid #efefef;border-radius:70px;height:30px;width:30px;">
                                5
                            </div>
                    </q-item-section>
                </q-item>

                <q-item clickable v-ripple>
                    <q-item-section avatar>
                        <q-icon name="delete_outline" />
                    </q-item-section>
                    <q-item-section>
                        Trash
                    </q-item-section>

                </q-item>
            </q-list>
            <div class="text-center q-mt-xl">
                <q-btn no-caps="" color="purple" @click="addTask()">
                    <q-icon name="add" class="q-mr-sm"></q-icon> New Task
                </q-btn>
            </div>
        </div>
        <div class="absolute-top" style="height: 150px;border:1px solid #f5f5f5;">
            <div class="text-center q-mt-xl">
                <q-icon name="content_paste" style="font-size:2em;" color="light-blue-5" />
                <div class="head-styl text-grey-6 q-mt-sm">
                    Todo List
                </div>
            </div>
        </div>
    </q-drawer>
    <q-page-container>
        <q-page padding>
            <div class="row">
                <q-select style="width:100%;padding:0;border:1px solid #f5f5f5;" outlined fill-input color="grey-2" use-input hide-selected hide-dropdown-icon bg-color="white" dense v-model="searchValue" behavior="menu" placeholder="Search Here ..">
                </q-select>
            </div>
            <div class="q-mt-sm">
                <q-list>
                    <q-item tag="label" v-for="list in listItems" v-bind:key="list.index" v-ripple class="q-pa-md" style="border:2px solid #f5f5f5;">
                        <q-item-section avatar style="margin-top:-40px;">
                            <q-radio v-model="listIndex" :val="list.index" color="primary" />
                        </q-item-section>
                        <q-item-section>
                            <q-item-label class="listName" :style="listIndex==list.index?'text-decoration: line-through;':''">{{list.name}}</q-item-label>
                            <q-item-label caption class="listDate q-mt-md" :style="listIndex==list.index?'text-decoration: line-through;':''">
                                {{list.date}}
                            </q-item-label>
                            <q-item-label caption class="listDesc q-mt-md" :style="listIndex==list.index?'text-decoration: line-through;':''">
                                {{list.desc}}
                            </q-item-label>
                        </q-item-section>
                        <q-item-section avatar style="margin-top:-40px;">
                            <div class="row">
                                <div class="q-mt-xs" v-if="list.isTrue">
                                    <q-icon name="info" color="orange-3" style="font-size:25px;"></q-icon>
                                </div>
                                <div class="q-mt-xs" v-if="!list.isTrue">
                                    <q-icon name="info" color="light-blue-3" style="font-size:25px;"></q-icon>
                                </div>
                                <div>
                                    <q-btn icon="more_vert" flat dense>
                                        <q-menu>
                                            <q-list>
                                                <q-item>
                                                    <q-item-section>Edit</q-item-section>
                                                </q-item>
                                                <q-item>
                                                    <q-item-section>Important</q-item-section>
                                                </q-item>
                                                <q-item>
                                                    <q-item-section>Delete</q-item-section>
                                                </q-item>
                                            </q-list>
                                        </q-menu>
                                    </q-btn>
                                </div>
                            </div>
                        </q-item-section>
                    </q-item>
                </q-list>
                <q-dialog v-model="isModal" class="q-pa-md">
                    <q-card>
                        <q-card-section class="row flex justify-between q-pb-none q-pa-lg">
                            <div>

                            </div>
                            <div class="head-styl text-grey-7">Add Task</div>
                            <div class="">
                                <q-btn icon="close" class="text-grey-6" flat round dense v-close-popup />
                            </div>
                        </q-card-section>

                        <q-card-section>
                            <div class="row ">
                                <div>
                                    <q-icon class="q-mt-sm" name="border_color" color="primary" style="font-size:20px;" />
                                </div>
                                <div class="q-ml-lg">
                                    <q-input v-model="taskName" outlined class="text-input" placeholder="Task" dense></q-input>
                                </div>
                            </div>
                            <div class="row q-mt-md">
                                <div>
                                    <q-icon class="q-mt-sm" name="description" color="primary" style="font-size:20px;" />
                                </div>
                                <div class="q-ml-lg">
                                    <q-editor class="text-input" placeholder="Compose an epic.." v-model="editor" :definitions="{
                                        upload: {
                                        tip: 'Upload to cloud',
                                        icon: 'insert_photo',
                                        handler: uploadIt
                                        }
                                    }" :toolbar="[
                                        ['bold', 'italic', 'underline'],
                                        ['upload']
                                    ]" />
                                </div>
                            </div>
                        </q-card-section>
                        <q-card-actions align="right">
                            <q-btn no-caps text-color="primary" @click="discard()">
                                Discard
                            </q-btn>
                            <q-btn no-caps color="primary" @click="saveTask()">
                                Add Task
                            </q-btn>

                        </q-card-actions>
                    </q-card>
                </q-dialog>
            </div>
        </q-page>
    </q-page-container>
</q-layout>
</template>

<script>
export default {
    name: 'Todo',
    data: function () {
        return {
            drawer: false,
            searchValue: null,
            listIndex: 0,
            taskName: '',
            listItems: [{
                    index: 0,
                    name: 'Meeting with Shaun Park at 4:50pm',
                    date: 'Aug, 07 2020',
                    desc: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua',
                    isTrue: false
                },
                {
                    index: 1,
                    name: 'Team meet at Starbucks',
                    date: 'Aug, 07 2020',
                    desc: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua',
                    isTrue: true
                },
                {
                    index: 2,
                    name: 'Meet Lisa to discuss project details',
                    date: 'Aug, 05 2020',
                    desc: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua',
                    isTrue: false
                },
                {
                    index: 3,
                    name: 'Download Complete',
                    date: 'Aug, 03 2020',
                    desc: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua',
                    isTrue: true
                }
            ],
            isModal: false,
            editor: ''
        }
    },
    methods: {
        addTask: function () {
            this.isModal = true;
        },
        discard: function () {
            this.isModal = false;
            this.editor = ''
            this.taskName = '';
        },
        saveTask: function () {
            this.isModal = false;
            this.editor = ''
            this.taskName = '';
        }
    }
}
</script>

<style scoped>
.head-styl {
    font-size: 1.6em;
    font-weight: bold;
    letter-spacing: 1px;
}

.menu-list .q-item {
    border-radius: 0 32px 32px 0;
}

.listName {
    font-size: 1.21em;
}

.listDate {
    font-size: .9em;
    color: darkseagreen;
    font-weight: bold;
}

.listDesc {
    width: 50vw;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    /* margin-left: -12px; */
}

.text-styl {
    font-size: 1.1em;
    font-weight: bold;
}

@media only screen and (max-width:400px) {
    .text-input {
        width: 40vw;
    }
}

@media only screen and (min-width:400px) {
    .text-input {
        width: 40vw;
    }
}

@media only screen and (min-width:420px) {
    .text-input {
        width: 60vw;
    }
}

@media only screen and (min-width:500px) {
    .text-input {
        width: 40vw;
    }
}

@media only screen and (min-width:700px) {
    .text-input {
        width: 42vw;
    }
}

@media only screen and (min-width:800px) {
    .text-input {

        width: 25vw;
    }
}

.q-item.q-router-link--active,
.q-item--active {
    background: #efefef;
    border-radius: 0 32px 32px 0;
}
</style>
