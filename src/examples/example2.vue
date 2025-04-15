<script setup>
import { ref } from "vue"
import GeometryView from "../components/MinimalisticGeometryView.vue"
import Slider from '../components/Slider.vue'
import Dropdown from "../components/Dropdown.vue"
import MetadataTextBox from "@/components/MetadataTextBox.vue"

//define path to grasshopper script
import def from "../assets/wallJJ.gh"
const path = def

// declare inputs here:
const heightSliderName = ref("Height")
const heightSliderValue = ref(17)

const widthSliderName = ref("Width")
const widthSliderValue = ref(33)

const verticalRotationSliderName = ref("Vertical Rotation")
const verticalRotationSliderValue = ref(0)

const horizontalRotationSliderName = ref("Horizontal Rotation")
const horizontalRotationSliderValue = ref(0)

const shapeTypeDropDownName = ref("Shape Type")
const shapeTypeIndex = ref(1)
const dropdownOptions = ref([
  {label: "Standard", value: 0},
  {label: "Concave", value: 1},
  {label: "Convex", value: 2}
])

let metadata = ref([])

//Put all inputs together to send them to Rhino.Compute
let inputs = ref({
  [heightSliderName.value]: heightSliderValue.value,
  [widthSliderName.value]: widthSliderValue.value,
  [verticalRotationSliderName.value]: verticalRotationSliderValue.value,
  [horizontalRotationSliderName.value]: horizontalRotationSliderValue.value,
  [shapeTypeDropDownName.value]: shapeTypeIndex.value
});

function updateValue(newValue, parameterName) {
  // Iterate over the inputs array
  for (const [key, value] of Object.entries(inputs.value)) {
    if (key == parameterName){
      inputs.value[key] = newValue
    }
  }
}

function receiveMetadata(newValue) {
  console.log(newValue)
  metadata.value = newValue
}

</script>

<template>

  <div id="sidebar">

    <Slider :title = heightSliderName
    min = "1"
    max = "20"
    step = "1"
    :value = heightSliderValue
    @update = "updateValue"
    ></Slider>

    <Slider :title = widthSliderName
    min = "1"
    max = "40"
    step = "1"
    :value = widthtSliderValue
    @update = "updateValue"
    ></Slider>

    <Slider :title = verticalRotationSliderName
    min = "1"
    max = "30"
    step = "1"
    :value = verticalRotationSliderValue
    @update = "updateValue"
    ></Slider>

    <Slider :title = horizontalRotationSliderName
    min = "1"
    max = "30"
    step = "1"
    :value = horizontalRotationSliderValue
    @update = "updateValue"
    ></Slider>

    <Dropdown :title=shapeTypeDropDownName
    :options=dropdownOptions
    :value=shapeTypeIndex
    @update = "updateValue"></Dropdown>

    <MetadataTextBox :metadata="metadata"></MetadataTextBox>
  
  </div>

  <div id="viewer">
    <GeometryView :data="inputs" :path="path" @updateMetadata="receiveMetadata"></GeometryView>
  </div>

</template>



<style scoped>

#sidebar {
  width: 350px;
  padding: 20px;
  flex-shrink: 0; 
}

#viewer { 
  width: 500px
}

</style>