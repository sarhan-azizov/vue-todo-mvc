<template>
    <Todo />
</template>

<script>

import Components from '@/components/index';

const { Todo } = Components;

export default {
  name: 'Home',
  components: {
    Todo,
  },
};
</script>
