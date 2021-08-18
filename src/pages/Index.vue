<template>
<q-page class="flex flex-center styl">
    <div class="">
        <q-stepper v-model="step" ref="stepper" contracted done-color="secondary" active-color="red" animated flat v-if="!isComplete">
            <q-step :name="1" prefix="1" :done="step > 1">
                <div class="container">
                    <div>
                        <span class="head">
                            Presentation
                        </span>
                    </div>
                    <span class="sub-head q-mt-sm">
                        Personal info
                    </span>
                </div>
                <div class="q-mt-md">
                    <q-input class="text-input" outlined v-model="text" label="First and Last Name" stack-label dense="" />
                </div>
                <div class="q-mt-md">
                    <q-input class="text-input" outlined type="email" v-model="email" label="Email" stack-label dense="" />
                </div>
                <div class="q-mt-md">
                    <q-input class="text-input" outlined v-model="phone" type="tel" label="Phone" stack-label dense />
                </div>
                <div class="q-mt-md">
                    <span style="font-size:1em;font-weight:bold;">Gender</span>
                    <q-option-group v-model="group" :options="gender" color="primary" inline />
                </div>
                <div class="q-mt-md">
                    <span style="font-size:1em;font-weight:bold;">Upload Resume</span><br>
                    <span style="font-size:.8em;">(File accepted: .pdf,.doc/.docx - Max file size: 150KB for demo limit) </span>
                    <q-file v-model="files" class="text-input q-mt-sm" label="The English Alphabet.pdf" outlined dense>
                        <template v-slot:prepend>
                            <q-icon name="cloud_upload" />
                        </template>
                    </q-file>
                </div>
                <q-separator class="q-mt-xl"></q-separator>
            </q-step>

            <q-step :name="2" caption="Optional" prefix="2" :done="step > 2">
                <div class="container">
                    <div>
                        <span class="head">
                            Work Availability
                        </span>
                    </div>
                    <span class="sub-head q-mt-sm">
                        Are you available for work?
                    </span>
                </div>
                <div class="q-mt-md">
                    <q-list>
                        <q-item tag="label" v-for="wrk in options" v-bind:key="wrk.index" dense v-ripple class="q-pa-md text-input q-mt-md" style="border:2px solid #f5f5f5;">
                            <q-item-section avatar>
                                <q-radio v-model="wrkIndex" :val="wrk.index" color="primary" />
                            </q-item-section>
                            <q-item-section>
                                <q-item-label>{{wrk.name}}</q-item-label>
                            </q-item-section>
                        </q-item>
                    </q-list>
                    <!-- <q-option-group class="text-input" v-model="wrkavailble" :options="options" color="green" type="radio" style="font-size:1.2em" /> -->
                </div>
                <div class="q-mt-md q-ml-md">
                    <span style="font-size:"> * Start branch radio based</span>
                </div>
                <q-separator class="q-mt-xl"></q-separator>
            </q-step>

            <q-step :name="3" prefix="3">
                <div>
                    <div class="container">
                        <div>
                            <span class="head">
                                Work Availability
                            </span>
                        </div>
                        <span class="sub-head q-mt-sm">
                            Additional info about "Part Time" availability
                        </span>
                    </div>
                    <div class="q-mt-md">
                        <span style="font-size:1em;font-weight:bold;">Minimum salary? (in USD)</span>
                        <q-select class="text-input" outlined behavior="menu" v-model="amtRange" :options="ranges" label="Choose a range" dense />
                    </div>
                    <div class="q-mt-md">
                        <span style="font-size:1em;font-weight:bold;">How soon would you be looking to start?</span>
                        <q-select class="text-input" outlined v-model="avbl" behavior="menu" :options="avblOptions" label="Choose your availability" dense />
                    </div>
                    <div class="q-mt-md">
                        <span style="font-size:1em;font-weight:bold;">When you prefer to work?</span>
                        <q-option-group v-model="workTym" :options="workTymOptions" color="primary" inline />
                    </div>
                </div>

                <q-separator class="q-mt-xl"></q-separator>
            </q-step>
            <template v-slot:navigation>
                <q-stepper-navigation class="flex justify-end">
                    <q-btn v-if="step > 1" color="negative" @click="$refs.stepper.previous()" label="Prev" class="q-mr-sm" />
                    <q-btn no-caps="" @click="save()" color="positive" :label="step === 3 ? 'Finish' : 'Next'" />
                </q-stepper-navigation>
            </template>
        </q-stepper>
        <div v-else>
            <div class="text-input">
                <div class="text-center">
                    <span class="head">Thanks your for your time test name !</span>
                </div>
                <div class="text-center q-mt-md">
                    <span class="para-styl">
                        We will contact you shortly at the following email address <b>test@yopmail.com</b>
                    </span>
                </div>
                <div class="text-center q-mt-xl">
                    <div style="font-weight:bold">
                        <q-checkbox v-model="agree" color="green" /> Please accept out <span style="color:green">Terms and conditions</span> before submit
                    </div>
                </div>
            </div>
            <q-separator class="q-mt-xl"></q-separator>
            <div class="flex justify-end q-mt-lg">
                <q-btn color="negative" @click="prev()" label="Prev" class="q-mr-sm" />
                <q-btn no-caps="" @click="submit()" color="positive" label="Submit" />
            </div>
        </div>
    </div>
</q-page>
</template>

<script>
export default ({
    name: 'PageIndex',
    data: function () {
        return {
            step: 1,
            text: '',
            email: '',
            phone: '',
            group: 'op1',
            wrkIndex: '',
            gender: [{
                    label: 'Male',
                    value: 'op1'
                },
                {
                    label: 'Female',
                    value: 'op2'
                },
            ],
            options: [{

                    name: 'Full time availability',
                    index: '1'
                },
                {
                    name: 'Part time availability',
                    index: '2'
                },
                {
                    name: 'Freelance / Contract availbility',
                    index: '3'
                }
            ],
            amtRange: null,
            ranges: [
                '<2k', '3-5k', '5-7k', '7-10k', '>10k'
            ],
            workTym: 'op1',
            workTymOptions: [{
                    label: 'Morning',
                    value: 'op1'
                },
                {
                    label: 'Afternoon',
                    value: 'op2'
                },
                {
                    label: 'No Preferences',
                    value: 'op3'
                },
            ],
            avbl: null,
            avblOptions: [
                'I can start immediately', 'I have some time'
            ],
            files: null,
            isComplete: false,
            agree: false,
        }
    },
    methods: {
        save: function () {
            if (this.step == 3) {
                this.isComplete = true;
                console.log("Saved !")
            } else {
                this.$refs.stepper.next();
            }
        },
        prev: function () {
            this.isComplete = false;
            this.step = 3;
        },
        submit: function () {
            this.$router.push("/todo")
        }
    }
    // setup() {
    //     return {
    //         step: ref(1)
    //     }
    // }
})
</script>

<style scoped>
.styl {
    font-family: ui-monospace;
}

.head {
    font-size: 2em;
    font-weight: bold;
}

.para-styl {
    font-size: 1rem;

}

.sub-head {
    font-size: 1.3em;
    font-weight: 600;

}

/* 
@media only screen and (max-width:335px) {
    .text-input {
        width: 95vw;
        height: 25vh;
    }
} */

@media only screen and (max-width:400px) {
    .text-input {
        width: 70vw;
    }
}

@media only screen and (min-width:400px) {
    .text-input {
        width: 70vw;
    }
}

@media only screen and (min-width:420px) {
    .text-input {
        width: 70vw;
    }
}

@media only screen and (min-width:500px) {
    .text-input {
        width: 70vw;
    }
}

@media only screen and (min-width:700px) {
    .text-input {
        width: 40vw;
    }
}
</style>
