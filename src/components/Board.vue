<template>
  <div
    class="board"
    :id="board.id"
    @dragover.prevent
    @drop.prevent="$emit('cardmoved', $event)"
  >
    <div
      class="board-top-padding board-special-borders"
      :class="background_selector(boards, board.id)"
    ></div>
    <header
      @dblclick="
        $emit('boardtitleedit', find_board_index_by_id(boards, board.id))
      "
      :class="{
        header_editing: board.isEdited,
      }"
    >
      <div class="text" v-if="!board.isEdited">{{ board.title }}</div>
      <div class="editing board-edit" v-if="board.isEdited">
        <form
          @submit.prevent="
            $emit(
              'saveboardtitle',
              $event,
              boards,
              find_board_index_by_id(boards, board.id)
            )
          "
          class="editing"
        >
          <input
            type="text"
            :id="'title-edit-' + board.id"
            :value="board.title"
            class="editing"
          />
          <input type="submit" style="display: none" class="editing" />
        </form>
      </div>
    </header>
    <div
      class="board-content board-special-borders"
      :class="background_selector(boards, board.id)"
    >
      <slot />
    </div>
  </div>
</template>

<script>
export default {
  props: ["board", "boards"],
  data() {
    return {};
  },
  methods: {
    background_selector(array, index) {
      return (
        "board-background-" +
        (array.findIndex((element) => {
          if (element.id === index) {
            return true;
          }
        }) %
          4)
      );
    },
    find_board_index_by_id(array, id) {
      return array.findIndex((element) => {
        if (element.id == id) {
          return true;
        }
      });
    },
  },
};
</script>

<style lang="scss" scoped>
$color-background: #caf3cb;
$color-board-1: #a3c19c;
$color-board-2: #99c99d;
$color-board-3: #97b788;
$color-board-4: #779a83;
$color-font: #5c5757;
$color-label: #eefeee;

.board {
  min-width: 250px;
  min-height: 90vh;
  margin: 0 0.5rem;

  .board-special-borders {
    $special-border: 12px;
    border-right: $special-border solid $color-background;
    border-left: $special-border solid $color-background;

    // main background for board
    // background: $color-board-1;
  }

  .board-background-0 {
    background: $color-board-1;
  }
  .board-background-1 {
    background: $color-board-2;
  }
  .board-background-2 {
    background: $color-board-3;
  }
  .board-background-3 {
    background: $color-board-4;
  }

  .board-top-padding {
    padding-top: 0.75rem;
  }

  .board-edit {
    input {
      padding: 0.45rem 0;
      width: 90%;
      border: none;
      outline: none;
      text-align: center;
      font-size: 18px;
      font-family: "Pacifico";
      background: $color-label;
    }
  }

  .header_editing {
    border: 3px solid rgb(120, 221, 255);
  }

  header {
    padding: 0.25rem 0;
  }

  .board-content {
    min-height: 90vh;
    padding-top: 0.75rem;
  }
}
</style>
