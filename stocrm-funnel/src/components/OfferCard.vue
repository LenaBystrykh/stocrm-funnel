<template>
    <div class="card" :style="{ 'border-left': `4px solid ${color}`}">
        <div class="card__header">
            <p class="card__id">#{{ card.id }}</p>
            <p class="card__city">{{ card.city }}</p>
        </div>
        <p class="card__name">{{ card.name }}</p>
        <p class="card__customer">{{ card.customer }}</p>
        <p class="card__segment">{{ card.segment }}</p>
    </div>
</template>

<script>
export default {
  name: 'OfferCard',
  props: {
    card: { type: Object, default: null },
    color: { type: String, default: "" }
  }
}
</script>

<style scoped>
.card {
    background-color: rgb(255, 255, 255);
    border-radius: 10px;
    padding: 8px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    margin: 8px 0;
}

.card__header {
    width: 100%;
    display: flex;
    justify-content: space-between;
    font-size: 14px;
    color: rgb(150, 150, 150);
}

.card__name {
    font-size: 16px;
    font-weight: bold;
    padding: 6px 0;
}

.card__segment {
    padding-top: 4px;
}
</style>