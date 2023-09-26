function cari(a, m, k){
	console.log("for a =",a,", m =",m,"and k =",k);
    
	let buatArray = [];

	for(let i=0; i < a.length; i++){
		buatArray.push(a.slice(i, a.length));
	}

	// console.log("buatArray: ", buatArray);

	let carikan = buatArray.filter(el=> {
		return el.length >= m;
	})

	// console.log(carikan);

	let kemungkinan = [];

	carikan.forEach(el => {
		// console.log(el);
		
		let test = el.slice(0, m);
		// console.log("test: ", test);
        // console.log("test: ", test.length);
        if(test.length === 4) {
            let test2 = el.slice(0, 1);
            // console.log("test: ", test2);
            let test3 = el.slice(2, 3);
            // console.log("test: ", test3);
            let test4 = el.slice(3, 4);
            // console.log("test: ", test4);

            let test5 = el.slice(3, 4);
            // console.log("test: ", test5);
            let test6 = el.slice(1, 2);
            // console.log("test: ", test6);
            let test7 = el.slice(0, 1);
            // console.log("test: ", test7);

            test2 = test2.concat(test3);
            // console.log(test2)
            test5 = test5.concat(test6);
            // console.log(test5)

            if(test2.reduce((acc,value)=> acc+value) === k || test5.reduce((acc,value)=> acc+value) === k ){
                kemungkinan.push(test2);
            }
        }
        else{
            if(test.reduce((acc,value)=> acc+value) === k ){
                kemungkinan.push(test);
            }
        }
	});

	console.log("Hasil = ",kemungkinan);
	return kemungkinan.length;

}
let a1 = [2,4,7,5,3,5,8,5,1,7];
let m1 = 4;
let k1 = 10;

let a2 = [15,8,8,2,6,4,1,7];
let m2 = 2;
let k2 = 8;

cari(a1, m1, k1);
cari(a2, m2, k2);
