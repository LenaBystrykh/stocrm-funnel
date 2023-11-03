<template>
    <div class="funnel">
        <div v-for="status in this.statuses" :key="status.id" class="funnel__column">
            <p class="funnel__status" :style="{ 'border-bottom': `4px solid ${status.color}`}">{{ status.title }}</p>
            <div class="funnel__cards">
                <div v-for="offer in getOffersForStatus(status.id)" :key="offer.id">
                    <OfferCard :card="offer" :color="status.color" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import OfferCard from './OfferCard.vue';

export default {
    name: 'FunnelPage',
    data() {
        return {
            statuses: [],
            offers: [],
        }
    },
    components: {
        OfferCard
    },
    methods: {
        getStatuses: function () {
            fetch('https://nastintesthodl.stocrm.ru/api/external/v1/offer/all_statuses?SID=10813_0c0a9a2f86eab09196705a274378b64a&BOARD_ID=1843')
                .then((response) => response.json())
                .then((json) => {
                    Object.values(json.RESPONSE).forEach(value => {
                        this.statuses.push({id: value.STATUS_ID, title: value.TITLE, color: value.FRONT_COLOR, sort: value.SORT});
                    })
                })
                .then(() => {
                    this.statuses.sort((a, b) => {
                        return a.sort - b.sort;
                    });
                })
        },
        getOffers: function () {
            fetch("https://nastintesthodl.stocrm.ru/api/external/v1/offers/get_from_filter", {
                method: "POST",
                body: JSON.stringify({
                    "SID":"10813_0c0a9a2f86eab09196705a274378b64a",
                    "FILTER":{"BOARD_ID":1843},
                    "SORT":{"DATE_CREATE":"ASC"},
                    "PAGE": 1,
                    "LIMIT": 99,
                }),
            })
            .then((response) => response.json())
            .then((json) => {
                json.RESPONSE.DATA.forEach(card => {
                    this.offers.push({
                        id: card.OFFER_ID,
                        status_id: card.OFFER_STATUS_ID,
                        name: card.CONTACT_TITLE,
                        customer: card.OFFER_CUSTOMER_NAME,
                        segment: card.SEGMENT_NAME,
                        city: card.CITY_NAME,
                    })
                })
            });
        },
        getOffersForStatus: function (status_id) {
            return this.offers.filter(offer => offer.status_id === status_id)
        }
    },
    mounted() {
        this.getStatuses();
        this.getOffers();
    }
}
</script>

<style scoped>
.funnel{
    display: flex;
    height: 100%;
}

.funnel__column {
    margin-right: 8px;
    flex: 1;
    height: 100%;
    display: flex;
    flex-direction: column;
}

.funnel__cards {
    flex: 1;
    overflow-y: scroll;
    -ms-overflow-style: none;
    scrollbar-width: none;
}
.funnel__cards::-webkit-scrollbar {
  display: none;
}

.funnel__column:last-of-type {
    margin-right: 0;
    flex: 1;
}

.funnel__status {
    text-align: center;
    font-weight: bold;
    background-color: white;
    padding: 12px 4px;
    border-radius: 10px;
}
</style>