<template>
    <div class="kanban-board">
        <div
            v-for="status in statuses"
            :key="status.name"
            class="kanban-column"
            @dragover.prevent
            @drop="onDrop(status.name, $event)"
        >
            <h2>{{ status.name }}</h2>
            <div
                v-for="card in status.cards"
                :key="card.id"
                class="kanban-card"
                draggable="true"
                @dragstart="onDragStart(card)"
            >
                <h3>{{ card.title }}</h3>
                <p>{{ card.description }}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            draggedCard: null,
            statuses: [
                {
                    name: 'To Do',
                    cards: [
                        { id: 1, title: 'Task 1', description: 'Description for task 1' },
                        { id: 2, title: 'Task 2', description: 'Description for task 2' },
                    ],
                },
                {
                    name: 'In Progress',
                    cards: [
                        { id: 3, title: 'Task 3', description: 'Description for task 3' },
                    ],
                },
                {
                    name: 'Done',
                    cards: [
                        { id: 4, title: 'Task 4', description: 'Description for task 4' },
                    ],
                },
            ],
        };
    },
    methods: {
        onDragStart(card) {
            this.draggedCard = card;
        },
        onDrop(statusName, event) {
            const status = this.statuses.find(status => status.name === statusName);
            const originalStatus = this.statuses.find(status =>
                status.cards.includes(this.draggedCard)
            );

            if (status && originalStatus) {
                originalStatus.cards = originalStatus.cards.filter(
                    card => card.id !== this.draggedCard.id
                );
                status.cards.push(this.draggedCard);
                this.draggedCard = null;
            }
        },
    },
};
</script>

<style scoped>
.kanban-board {
    display: flex;
    gap: 20px;
    padding: 20px;
}

.kanban-column {
    background-color: #f4f4f4;
    padding: 10px;
    border-radius: 5px;
    width: 300px;
}

.kanban-card {
    background-color: #ffffff;
    border: 1px solid #dddddd;
    border-radius: 5px;
    padding: 10px;
    margin-bottom: 10px;
    cursor: move;
}
</style>
