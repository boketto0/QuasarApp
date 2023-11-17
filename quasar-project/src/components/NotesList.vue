<template>
  <div>
    <p>{{ title }}</p>
    <div class="q-gutter-md">
      <q-card
        v-for="todo in todos"
        :key="todo.id"
        @click="increment"
        class="q-mb-md"
      >
        <q-card-section>
          {{ todo.content }}
          <q-btn
            @click.stop="removeNote(todo.id)"
            icon="delete"
            label=""
            class="q-ml-md"
          />
        </q-card-section>
      </q-card>
    </div>
    <p>Count: {{ todoCount }} / {{ meta.totalCount }}</p>
    <q-input v-model="newNoteContent" label="Текст заметки" />
    <q-btn
      @click="addNewNote"
      icon="add"
      label="Добавить заметку"
      class="q-mt-md"
    />
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  PropType,
  computed,
  ref,
  toRef,
  Ref,
} from 'vue';
import { Todo, Meta } from './models';

function useClickCount() {
  const clickCount = ref(0);
  function increment() {
    clickCount.value += 1;
    return clickCount.value;
  }

  return { clickCount, increment };
}

function useDisplayTodo(todos: Ref<Todo[]>) {
  const todoCount = computed(() => todos.value.length);
  return { todoCount };
}

export default defineComponent({
  name: 'NotesList',
  props: {
    title: {
      type: String,
      required: true
    },
    todos: {
      type: Array as PropType<Todo[]>,
      default: () => []
    },
    meta: {
      type: Object as PropType<Meta>,
      required: true
    },
    active: {
      type: Boolean
    }
  },
  setup (props, { emit }) {
    const { clickCount, increment } = useClickCount();
    const { todoCount } = useDisplayTodo(toRef(props, 'todos'));
    const newNoteContent = ref('');

    function addNewNote() {
      if (newNoteContent.value.trim() !== '') {
        emit('addNote', {
          id: 0,
          content: newNoteContent.value
        });
      
        newNoteContent.value = '';
      }
    }

    function removeNote(id: number) {
      emit('removeNote', id);
    }

    return { clickCount, todoCount, newNoteContent, addNewNote, removeNote };
  },
});
</script>

<style scoped lang="scss">
.q-gutter-md {
  display: flex;
  flex-wrap: wrap;
  margin-right: 8px;
  width: 800px;
}

.q-mb-md {
  margin-bottom: 4px;
}
</style>
