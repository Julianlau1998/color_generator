<template>
    <div class="is-topnav p-3 pb-4 pt-4">
        <div class="columns is-mobile">
            <div class="column ml-4">
                <span>
                    Color Generator
                </span>
            </div>
            <!-- <div class="column settingsWrapper">
                <div v-if="settings" class="settingsItems settings">
                    <label class="label is-pointer setting noselect">
                        <input @change="fileInput" accept=".txt, .md" type="file" required/>
                        <span>
                            Import File
                        </span>
                    </label>
                </div>
                <i
                    class="fas fa-ellipsis-v settings-icon is-pointer"
                    @click="settings=!settings"
                />
            </div> -->
        </div>
    </div>
</template>

<script>
export default {
    name: 'TopNav',
    data () {
        return {
            settings: false,
            shareAvailable: false,
            playBillingSupported: false
        }
    },
    created () {
        this.checkPlayBillingAvailable()
        if(navigator.share !== undefined) {
            this.shareAvailable = true
        }
    },
    methods: {
        share () {
            this.$emit('share')
        },
        fileInput (event) {
            const file = event.target.files[0];
            const reader = new FileReader();
            reader.onload = e => this.$emit('fileInput', e.target.result);
            reader.readAsText(file);
            this.settings = false
        },
        async checkPlayBillingAvailable () {
            if ('getDigitalGoodsService' in window) {
                const service = await window.getDigitalGoodsService('https://play.google.com/billing');
                if (service) {
                    this.playBillingSupported = true
                }
            }
        },
        async makePurchase(service) {
            const paymentMethods = [{
                supportedMethods: "https://play.google.com/billing",
                data: {
                    sku: 'support',
                }
            }]
            const paymentDetails = {
                total: {
                    label: `Total`,
                    amount: {currency: `USD`, value: `10`}
                }
            }
            const request = new PaymentRequest(paymentMethods, paymentDetails);
            try {
                const paymentResponse = await request.show();
                const {purchaseToken} = paymentResponse.details;
                await service.acknowledge(purchaseToken, 'repeatable');
            } catch(e) {
                alert('Something went wrong. Please try again.')
            }
        }
    }
}
</script>