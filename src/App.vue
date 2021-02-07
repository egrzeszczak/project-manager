<template>
  <div id="app" @click="global_click">
    <button @click="new_board">+ Board</button>
    <button @click="new_card(0)">+ Card</button>

    <header class="header-padding">
      Planer dnia
    </header>

    <div class="planner">
      <Board
        v-for="(board, board_index) in boards"
        :key="board_index"
        :board="board"
        :boards="boards"
        @cardmoved="move_card"
        @boardtitleedit="edit_board"
        @saveboardtitle="edit_title"
      >
        <Card
          v-for="(card, card_index) in board.cards"
          :key="card_index"
          :card="card"
          @carddragstart="card_drag_start"
          @carddragend="card_drag_end"
        >
        </Card>
      </Board>
    </div>
  </div>
</template>

<script>
import Board from "./components/Board";
import Card from "./components/Card";
export default {
  components: {
    Board,
    Card,
  },
  data() {
    return {
      boards: [],
    };
  },
  methods: {
    new_board() {
      this.boards.push({
        id: Date.now(),
        title: "Tablica",
        isEdited: false,
        cards: [],
      });
    },
    new_card(board_index) {
      this.boards[board_index].cards.push({
        id: Date.now(),
        content: "Zadanie",
        isEdited: false,
        isDraggable: true,
      });
    },
    card_drag_start(e) {
      const card_dragged_id = e.target.id;
      e.dataTransfer.setData("card_dragged_id", card_dragged_id);
      console.log(`Dragging ${card_dragged_id}`);
    },
    card_drag_end(e) {
      const card_dropped_id = e.target.id;
      console.log(`Dropping ${card_dropped_id}`);
    },
    new_card_assign(index, card) {
      this.boards[index].cards.push(card);
    },
    move_card(e) {
      const card_received_id = e.dataTransfer.getData("card_dragged_id");
      let board_receiving_id = e.target.id;

      if (!(e.target.classList.value == "board")) {
        let the_impostor = e.target;
        while (
          (the_impostor = the_impostor.parentElement) &&
          !the_impostor.classList.value == "board"
        ) {
          console.log(the_impostor);
        }
        board_receiving_id = the_impostor.id;
      }

      // Find which index of board_receiving_id
      let board_id = null;
      for (let i = 0; i < this.boards.length; i++) {
        if (this.boards[i].id == board_receiving_id) board_id = i;
      }

      // Find the object, get it and move it to correct board
      for (let i = 0; i < this.boards.length; i++) {
        for (let j = 0; j < this.boards[i].cards.length; j++) {
          if (this.boards[i].cards[j].id == card_received_id) {
            // j is the index to delete
            let card_retrieved = this.boards[i].cards.splice(j, 1)[0];
            this.new_card_assign(board_id, card_retrieved);
          }
        }
      }

      console.log(
        `${card_received_id} is being moved to ${board_receiving_id} (index: ${board_id})`
      );
    },
    edit_board(board_index) {
      this.boards[board_index].isEdited = !this.boards[board_index].isEdited;
      setTimeout(() => {
        document
          .getElementById(`title-edit-${this.boards[board_index].id}`)
          .focus();
      }, 0);
      this.refresh();
    },
    edit_title(e, boards, index) {
      this.boards[index].title = String(e.target[0].value);
      console.log(e.target[0].value);
      console.log(index);
      this.edit_board(index);
    },
    global_click(e) {
      if (!e.target.classList.value.includes("editing")) {
        for (let i = 0; i < this.boards.length; i++) {
          this.boards[i].isEdited = false;
          for (let j = 0; j < this.boards[i].cards.length; j++) {
            this.boards[i].cards[j].isEdited = false;
            this.boards[i].cards[j].isDraggable = true;
          }
        }
      }
    },
    // ref
    refresh() {
      this.boards.push({});
      this.boards.pop();
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Pacifico&display=swap");

$color-background: #caf3cb;
$color-board-1: #a3c19c;
$color-board-2: #99c99d;
$color-board-3: #97b788;
$color-board-4: #779a83;
$color-font: #5c5757;
$color-label: #eefeee;

html,
body {
  padding: 0;
  margin: 0;
}

#app {
  background: $color-background;

  header {
    background: $color-label;
    color: $color-font;
    border-radius: 1rem;
    // padding: 0.5rem;
    text-align: center;
    font-family: "Pacifico";
    font-size: 24px;
  }
  .header-padding {
    margin: 1.5rem 15% 3rem 15%;
    font-size: 48px;
  }
  .planner {
    display: flex;
    justify-content: center;
  }
}
</style>
