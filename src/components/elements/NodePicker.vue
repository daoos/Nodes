<template>
  <div class="nodePicker">
    <div class="window">
      <h2>Add Node</h2>
      <div class="nodes">
        <div class="node" v-for="(nodeType, index) in types" :key="index" :index="index" @click="addNode(index)">
            <h3>{{ nodeType.name }}</h3>
            <p>{{ nodeType.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NodePicker',
  data () {
    return {
      types: [{name: 'Display', description: 'Prints out any number you feed in.', template: { type: 'display', inputs: [{ id: 0, name: 'In' }], title: 'Display' }},
        {name: 'Input', description: 'Lets you enter a number.', template: { type: 'input', outputs: [{ id: 0, name: 'Out' }], title: 'Input', value: 1 }},
        {name: 'Slider', description: 'A 0-1 interactive slider.', template: { type: 'slider', inputs: [], outputs: [{ id: 0, name: 'Out' }], title: 'Slider', value: 0.5 }},
        {name: 'Add', description: 'Returns a + b.', template: { type: 'add', inputs: [{ id: 0, name: 'A' }, { id: 1, name: 'B' }], outputs: [{ id: 0, name: 'Out' }], title: 'Add', symbol: '+' }},
        {name: 'Subtract', description: 'Returns a − b.', template: { type: 'subtract', inputs: [{ id: 0, name: 'A' }, { id: 1, name: 'B' }], outputs: [{ id: 0, name: 'Out' }], title: 'Subtract', symbol: '-' }},
        {name: 'Multiply', description: 'Returns a × b.', template: { type: 'multiply', inputs: [{ id: 0, name: 'A' }, { id: 1, name: 'B' }], outputs: [{ id: 0, name: 'Out' }], title: 'Multiply', symbol: '×' }},
        {name: 'Divide', description: 'Returns a ÷ b.', template: { type: 'divide', inputs: [{ id: 0, name: 'A' }, { id: 1, name: 'B' }], outputs: [{ id: 0, name: 'Out' }], title: 'Divide', symbol: '÷' }},
        {name: 'Power', description: 'Returns a ^ b.', template: { type: 'power', inputs: [{ id: 0, name: 'A' }, { id: 1, name: 'B' }], outputs: [{ id: 0, name: 'Out' }], title: 'Power', symbol: 'aⁿ' }}]

    }
  },
  methods: {
    addNode: function (e) {
      // We convert to JSON to unbind the template, else every new node will be linked.
      // This is just a cheap and dirty way to do deep copy.
      let newNode = JSON.parse(JSON.stringify(this.types[e].template))
      newNode.position = {x: 100, y: 100}
      newNode.id = UUID()
      newNode.compact = false
      this.$store.commit('addNode', newNode)
      this.$parent.isPickerVisible = false
    }
  }
}

function UUID () {
  var out = ''
  var chars = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ'
  for (var i = 0; i < 6; i++) {
    out += chars.charAt(Math.floor(Math.random() * chars.length))
  }
  return out
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.nodePicker {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: #040505CC;
}

.nodePicker .window {
  background-color: #303133;
  width: 615px;
  height: 300px;
  border-radius: 10px;
  color: white;
  padding: 15px;
  position: absolute;
  top: 50%;
  left: 50%;

  margin: -150px 0 0 -300px;
}

.nodePicker .nodes {
  overflow-x: hidden;
  margin-top: 10px;
  height: 200px;
}

.nodePicker .node {
  display: inline-block;
  width: 270px;
  background-color: #202123;
  border-radius: 5px;
  padding: 10px;
  text-align: left;
  cursor: pointer;
  margin: 5px;
}

.nodePicker .node:hover {
  background-color: #252628;
}

.nodePicker .node p {
  color: #BCC;
}
</style>
