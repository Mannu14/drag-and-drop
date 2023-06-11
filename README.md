<!DOCTYPE html>
<html>
<head>
<title>Drag and Drop Interface</title>
<style>
  *{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  display: flex;
  align-items: center; 
  justify-content: center;
  min-height: 100vh;
  background: #fff;
}
.container {
    display: inline-block;
    margin: 10px;
    padding: 10px;
    border: 1px solid black;
  }
  
.drag-container {
  min-height: 100px;
  border: 1px dashed gray;
  padding: 10px;
  margin-top: 10px;
  width: 220px;
}
  
.item {
  background-color: lightblue;
  border: 1px solid blue;
  padding: 5px;
  margin: 5px;
  cursor: move;
  width: 80px;
}
.item img{
  width: 70px;
  height: 70px;
}
  
.success-message {
  color: green;
  font-weight: bold;
}
.drag-area{
    border: 2px dashed #fff;
    height: 210px;
    width: 200px;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
}
.drag-area.active{
    border: 2px solid #fff;
}
.drag-area .icon{
    font-size: 100px;
    color: #fff;
}
.drag-area header{
    font-size: 30px;
    font-weight: 500;
    color: blue;
}
.drag-area span{
    font-size: 25px;
    font-weight: 500;
    color: #fff;
    margin: 10px 0px 15px 0;
}
.drag-area img{
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: 5px;
}
#resetButton{
  margin-left: 70px;
  margin-top: 10px;
  padding: 10px 20px;
  border-radius: 7px;
  border: none;
  background: white;
  cursor: pointer;
  background: rgb(51, 51, 161);
  transition: all 0.7s;
}
#resetButton:hover{
  background: blue;
}
</style>
</head>
<body>
<div class="container">
    <h2>Container 1</h2>
    <div id="container1" class="drag-container">
      1.<div class="item" draggable="true"> <img src="image/M Name - Manish.jpg"></div>
      2.<div class="item" draggable="true"> <img src="image/ManishName.jpeg"></div>
      3.<div class="item" draggable="true"> <img src="image/R.jpg"></div>
    </div>
  </div>

  <div class="container">
   <h2>Container 2</h2>
  <div id="container2" class="drag-container">
   <div class="drag-area"> 
       <header>Drag & Drop to Upload File</header>
       <input type="file" hidden>
   </div>
  </div>
   <button id="resetButton">Reset</button>
</div>

<script>
document.addEventListener("DOMContentLoaded", function() {
const container1 = document.getElementById("container1");
const container2 = document.getElementById("container2");
const resetButton = document.getElementById("resetButton");
let successMessage = null;

container1.addEventListener("dragstart", function(event) {
  event.dataTransfer.setData("text/plain", event.target.id);
  event.target.classList.add("dragging");
});



const dropArea = document.querySelector(".drag-area"),
dragText = dropArea.querySelector("header"),
button = dropArea.querySelector("button"),
input = dropArea.querySelector("input");

let file;

input.addEventListener("change", function(){
  file = this.files[0];  // only one file select
  showFile();
  dropArea.classList.add("active");
})


// If user Drag file over DropArea
dropArea.addEventListener("dragover", (event)=>{
  event.preventDefault(); 
  dropArea.classList.add("active");
  dragText.textContent = "Release to Upload File";
});

// If user leave dragged file from DropArea
dropArea.addEventListener("dragleave", ()=>{
  dropArea.classList.remove("active");
  dragText.textContent = "Drag & Drop to Upload File";
});

// If user drop file on DropArea
dropArea.addEventListener("drop", (event)=>{
  event.preventDefault(); 
  file = event.dataTransfer.files[0];  // if user select multiple images then we will select only first
  showFile();
  showSuccessMessage();
});

function showFile(){
  let fileType = file.type;
    
  let validExtension = ["image/jpeg", "image/jpg", "image/png"];
  if(validExtension.includes(fileType)){  // if user selected file is an image file
      let fileReader = new FileReader();  // creating new FileReader object
      fileReader.onload = () =>{
       let fileURL = fileReader.result;  // passing user file source in fileURL variable
       let imgTag = `<img src="${fileURL}" alt"">`;  // creating an img tag and passing user selected file
       dropArea.innerHTML = imgTag;
      }
      fileReader.readAsDataURL(file);
  }
  else{
    alert("This is not an Image File!");
    dropArea.classList.remove("active");
  }
}

resetButton.addEventListener("click", function() {
  container1.innerHTML = `
    <div class="item" draggable="true"><img src="image/M Name - Manish.jpg"></div>
    <div class="item" draggable="true"><img src="image/ManishName.jpeg"></div>
    <div class="item" draggable="true"><img src="image/R.jpg"></div>
  `;
  container2.innerHTML = "";
  removeSuccessMessage();
});

function showSuccessMessage() {
  if (!successMessage) {
    successMessage = document.createElement("p");
    successMessage.classList.add("success-message");
    successMessage.textContent = "Item successfully dropped!";
    container2.parentNode.insertBefore(successMessage, container2.nextSibling);
  }
}

function removeSuccessMessage() {
  if (successMessage) {
    successMessage.parentNode.removeChild(successMessage);
    successMessage = null;
  }
}
});

  </script>
</body>
</html>
