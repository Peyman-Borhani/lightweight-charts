<script>  
// @ts-nocheck
  import {LineStyle}  from  'lightweight-charts';
  import {Chart, LineSeries, PriceLine}  from  'svelte-lightweight-charts';
	
  import  data   from   './assets/data.js';

  const options = {
        width:  800,
        height: 480,
        layout: {  textColor: '#d1d4dc',
								   backgroundColor: '#001030dd',
        },
        rightPriceScale: { 
					           scaleMargins: {top: 0.3, bottom: 0.25}
        },
        crosshair: {
            vertLine: {  width: 4,
											   color: 'rgba(224, 227, 235, 0.1)',
											   style: 0,
            },
            horzLine: {  visible: false,
											   labelVisible: true }
        },
        grid: {  vertLines: {color: '#8883'},
							   horzLines: {color: '#0128'}
        },
        handleScroll: { vertTouchDrag: false }
    }
	
   // min/avg/max values
    let minPrice = data[0].value;
    let maxPrice = minPrice;
	  let avgPrice = 0;
    for(let d of data) {
			    const price = d.value;
          avgPrice+= price;
					if (price>maxPrice)  maxPrice = price;
			    
			    if (price<minPrice)  minPrice = price;
    }
    avgPrice = avgPrice / data.length;

	
	  const ref= e=>{ e?.timeScale().fitContent() }

    let container, t = false;

	  function resizer(e) { t=!t;
                          options.width = container.clientWidth;
                          options.height= container.clientHeight;
		}
    window.onload= ()=>resizer();
    //console.log(...data)
</script>


<svelte:window on:resize={resizer} />

<main>

 <h1> Price lines with titles </h1>
 {#key t}
  <div id='container'   bind:this={container}>
  <Chart {...options}  {ref} >
       <LineSeries  {data}
									  color="#adfb"
									  lineWidth={1.8}
									  crosshairMarkerVisible={false}
									  lastValueVisible={false}
									  priceLineVisible={false}
       >
            <PriceLine  title="Min"
                        price={minPrice}
											  color="darkred"
											  lineWidth={1}
											  lineStyle={LineStyle.Solid}
											
											  axisLabelVisible={true} />
					
            <PriceLine  title="Mid"
											  price={avgPrice}
											  color="#358"
											  lineWidth={1.4}
											  lineStyle={LineStyle.Dotted}
											  axisLabelVisible={true}
            />
            <PriceLine  title="Max"
											  price={maxPrice}
											  color="darkgreen"
											  lineWidth={1}
											  lineStyle={LineStyle.Solid}
											  axisLabelVisible={true}
            />
        </LineSeries>
    </Chart>
  </div>
 {/key}

</main>

<style>
	:root {  font-family: -apple-system, BlinkMacSystemFont, 
		                    'Segoe UI', Roboto, Oxygen, Ubuntu, 
		                     Cantarell, 'Open Sans', 'Helvetica Neue'
		                    , sans-serif;
}
	:global(body){  margin:  0;       overflow: hidden;
                  padding: 0;       background-color: #def;
                  font-size: 2vw;                  box-shadow: inset 0 0 16ch #000;	               
}
	:global(div)  { border-radius: 1vmin;
	                box-shadow: inset 0 0 9ch #000
                }
	
	main     {  display: flex;      flex-direction: column;
              width:  100vw;      justify-content: center;
              height: 100vh;      align-items: center;
                                  background-color: silver;
}


	#container  { width:  94%;      box-shadow:  0 0 3ch #000; 
                height: 70vh;     outline: inset 4pt #000;
                                  border-radius: 1vmin;
                
}
	h1 {  font-weight: 400;
        line-height: 1.1;
} 
</style>