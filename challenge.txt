var fs = require('fs');
 
function readdirAsync(file) {
  return new Promise(function (resolve, reject) {
    fs.readFile(file, 'utf8', function (error, data) {
      if (error) {
        reject(error);
      } else {
        resolve(data);
      }
    });
  });
}

function output(data){

  var input = data.split('\n');
  var lineOne = input[0].split(' ');
  for (var i = 0; i< lineOne.length; i++) {
    lineOne[i] = parseInt(lineOne[i], 10);
  }



  var lineTwo = input[1].split(' ');
  for (var i = 0; i< lineTwo.length; i++) {
    lineTwo[i] = parseInt(lineTwo[i], 10);
  }



  numDays= lineOne[0];
  windowSize= lineOne[1];
  data = lineTwo;

  if (1 <= numDays && numDays <= 200000 && 1 <= windowSize && windowSize <= numDays){


  	subranges(numDays, windowSize, data);
  	console.log(results);

	}

  else {

  	console.log(error);

	}




}




function subranges (numDays, windowSize, data) {
	 results = [];

 for (var i = 0; i <= numDays-windowSize; i++){
 
			var isIncrease = true;
			var rangeStart = i;
			var rangeStop = i;
			var windowSubRanges = 0;


		for (var j = i; j <= i + windowSize - 1; j++){

				//if next index is last of the loop
				if (j === i + windowSize - 1){

					//set rangeStop to last index
					rangeStop = j;

					

					//calculate subranges between rangeStart and rangeStop...if isIncrease is false, multiply by -1
					if (isIncrease=== false){

						windowSubRanges -= (rangeStop-rangeStart) * (rangeStop-rangeStart + 1) / 2;

					}


					else {

						windowSubRanges += (rangeStop-rangeStart) * (rangeStop-rangeStart + 1) / 2;



					}
						


					
				}






			//else, check if current index is increasing from previous (isIncrease flag)

				else if (isIncrease === true){

				//if yes, check if next index greater than current index?
					if (data[j]<data[j + 1]){

					//if yes, reassign rangeStop to point at next index
					rangeStop = j + 1;
					// console.log(rangeStop);
					
					}

					//if next index is same as current
					else if (data[j]===data[j + 1]){
	
					
					windowSubRanges += (rangeStop-rangeStart) * (rangeStop-rangeStart + 1) / 2;
					

					
					rangeStart= j+1;
					rangeStop= j+1;
				
					}





					//if no, we have hit the end of the increasing subranges
					else {
						//set isIncrease flag to false
						isIncrease= false;
						// calculate # of subranges between rangeStart and rangeStop, add this value to windowSubRanges 
						windowSubRanges += (rangeStop-rangeStart) * (rangeStop-rangeStart + 1) / 2;
						

						//start new subrange by reassigning rangeStart to current index
						rangeStart= j;
						rangeStop= j+1;

							}
						
						}


					

				//if no, current index is less than previous, check if next index is also less than current
				else {
					if (data[j]>data[j + 1]){
					// if yes, reassign rangeStop to point at next index
					rangeStop = j + 1;
					}

					//if next index is same as current
					else if (data[j]===data[j + 1]){
		
						// calculate # of subranges between rangeStart and rangeStop, add this value to windowSubRanges 
						windowSubRanges -= (rangeStop-rangeStart) * (rangeStop-rangeStart + 1) / 2;
						

						//start new subrange by reassigning rangeStart to current index
						rangeStart= j+1;
						rangeStop= j+1;
					
					}






					//if no, we have hit the end of the decreasing subranges
					else{
						//set isIncrease flag to true
						isIncrease= true;
						//calculate # of subranges between rangeStart and rangeStop, add this value multiplied by -1 to windowSubRanges
						windowSubRanges -= (rangeStop-rangeStart) * (rangeStop-rangeStart + 1) / 2;
						
						//start new subrange by reassigning rangeStart to current index
						rangeStart= j;
						rangeStop= j+1;
						}
					}

					
						
				}


		//push the subranges for the current window (windowSubRanges) to results array

		results.push(windowSubRanges);
	
	}


	
	}











//FILE NAME GOES HERE
readdirAsync('input.txt')
  .then(function(data) {
  output(data);

}).catch(function(err) {
  if (err) {
    console.log('err', err);
  };
});