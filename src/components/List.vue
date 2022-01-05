<template>
  <div class="row mt-3">
    <div class="col-10">
      <ul class="list-group">
        <li class="list-group-item" v-for="(todo, index) in todos" :key="index">
          <div class="row d-flex d-flex align-items-center">
            <div class="col-10">
              <span v-if="todo.status">
                <del>{{todo.activity}}</del>
              </span>
              <span v-else>
                {{todo.activity}}
              </span>
            </div>
            <div class="col-2 gap-2 d-flex justify-content-end">
              <button class="btn btn-success " @click="done(index)">✓</button>
              <button class="btn btn-danger " @click="destroy(index)">X</button>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      default: []
    }
  },
  setup(props, { emit }) {
    const destroy = index => {
      emit('destroy', index)
    }
    const done = index => {
      emit('done', index)
    }
    return { destroy, done }
  }
}
</script>