---
title: RREF
toc: false
---

# RREF Calculator
<style>
  

table {
  table-layout: fixed; 
  width: auto;
  margin: 0 auto;
  border: 0px solid ; 
  border-spacing: 0px 3px ; 
  border-collapse: separate ; 

}

th, td {
 width: 100px ;   
 border-top: 1px solid;
 border-bottom: 1px solid ; 
 border-left: .5px solid ; 
 border-right: .5px solid ; 
 border-radius: 2px ; 
 padding: 2px ;   
 background-color: #2b2a33 ; 
}


</style>

```js
var rows = Inputs.number([1,10],{width: "100px" , value: 1, label: "Rows:"}) ; 
var cols = Inputs.number([1,10],{width: "100px" ,value: 1, label: "Cols:"}) ; 
var nrows = view(rows) ; 
var ncols = view(cols) ; 
```
<div class="grid grid-cols-2">
  <div class="card">${rows}</div>
  <div class="card">${cols}</div>
</div>



<div class="grid">
<div class="card" style="margin: auto;">
<h2> Input Matrix </h2>
${matrix}
</div>
</div>

```js
var inputs_array = d3.range(nrows*ncols).map(d=>Inputs.number([-100,100],{value:0,  step:.01, width:"100px;"}))

/* for (let r=0;r<nrows; r+=1) {
    for (let c=0;c<ncols; c+=1) {
      var j = 10*r + c ; 
      inputs_array[j][0].disabled=false
    }
}
 */
var matrix = Inputs.form(
  inputs_array,
  {
    template(inputs) {
      return htl.html`<div style="display:grid;grid-template-columns:repeat(${ncols}, 100px)">${inputs}</div>`;
    }
  }
); 
var matrix_values = view(matrix) ; 
```




```js
var m = list_to_matrix(matrix_values,nrows,ncols) ; 
var mcopy = m.map(arr => arr.slice()) ; 
var n = rowReduce(mcopy) ; 
var spot = document.getElementById("result")
spot.innerHTML = maketable(n,nrows,ncols)
```
<div class="grid">
<div class="card" style="margin: auto;">
<h2> Row Reduced Echelon Form</h2>
<div id="result">
</div>
</div>
</div>

```js
function list_to_matrix(data,rows,columns) {
  var result = [];
  for (var r = 0; r < rows; r++) {
    result.push(data.slice(r*columns, r*columns+columns));
  }
  return result;
}
```
```js
function rowReduce(matrix) {
    const numRows = matrix.length;
    const numCols = matrix[0].length;
    let pivotRow = 0;
    for (let col = 0; col < numCols && pivotRow < numRows; col++) {
        let pivotFound = false;
        for (let row = pivotRow; row < numRows; row++) {
            if (matrix[row][col] !== 0) {
                pivotFound = true;
                if (row !== pivotRow) {
                    [matrix[pivotRow], matrix[row]] = [matrix[row], matrix[pivotRow]]; // Swap rows  
                }
                // Normalize the pivot row
                const pivot = matrix[pivotRow][col];
                for (let j = col; j < numCols; j++) {
                    matrix[pivotRow][j] /= pivot;
                }
                // Eliminate all other elements in the current column
                for (let i = 0; i < numRows; i++) {
                    if (i !== pivotRow) {
                        const factor = matrix[i][col];
                        for (let j = col; j < numCols; j++) {
                            matrix[i][j] -= factor * matrix[pivotRow][j];
                        }
                    }
                }
                pivotRow++; // Move to the next row for the next pivot
                break; // Move to the next column
            }
        }
        if (!pivotFound) {

        }
    }
    return matrix;
}
```

```js
function maketable(data, rows, cols) {
  var s = `<table>\n`
  for (let r=0;r<nrows; r+=1) {
    s+="<tr>"
    for (let c=0;c<ncols; c+=1) {
        s+=`<td>${data[r][c].toFixed(3)}</td>`
      }
    s+=`</tr>\n`
  }
  s+=`</table>\n`
  return s

}
```