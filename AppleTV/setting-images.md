---
layout: default
title: Setting images
---

Right now the blue squares are pretty boring. Let's spice some things up by adding some Spanish modernist images to our scene. 

## The image data 

We'll first need to import the source paths for our images. We can pretend we are grabbing this data from the **Francisco  de Goya API** that serves up this nicely formatted array of image paths of the **Los Caprichos** etchings.

Open up the `galleryData.js` file you created earlier and paste the code snippet below into it. 

	  var imageData = [
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._01_-_Autorretrato._Francisco_Goya_y_Lucientes2C_pintor_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._02_-_El_si_pronuncian_y_la_mano_alargan_al_primero_que_llega_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._03_-_Que_viene_el_Coco_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._04_-_El_de_la_rollona_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._05_-_Tal_para_qual_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._06_-_Nadie_se_conoce_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._07_-_Ni_asi_la_distingue_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._09_-_Tantalo_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._10_-_El_amor_y_la_muerte_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._11_-_Muchachos_al_avC3ADo_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._12_-_A_caza_de_dientes_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._13_-_Estan_calientes_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._14_-_Que_sacrificio21_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._15_-_Bellos_consejos_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._16_-_Dios_la_perdone_-_Y_era_su_madre_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._17_-_Bien_tirada_estC3A1_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._18_-_Y_se_le_quema_la_Casa_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._19_-_Todos_CaerC3A1n_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._20_-_Ya_van_desplumados_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._21_-_C2A1Qual_la_descaC3B1onan21_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._22_-_Pobrecitas21_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._23_-_Aquellos_polbos_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._24_-_No_hubo_remedio_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._25_-_Si_quebrC3B3_el_Cantaro_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._26_-_Ya_tienen_asiento_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._27_-_Quien_mC3A1s_rendido3F_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._28_-_Chiton_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._29_-_Esto_si_que_es_leer_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._30_-_Porque_esconderlos3F_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._31_-_Ruega_por_ella_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._32_-_Por_que_fue_sensible_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._33_-_Al_Conde_Palatino_thumb.jpg',
        'http://demo.famo.us.s3.amazonaws.com/hub/apps/carousel/Museo_del_Prado_-_Goya_-_Caprichos_-_No._34_-_Las_rinde_el_SueC3B1o_thumb.jpg'
    ];

_Note that this is just a pretend API. If we were working with a real API we would need to wait for our API call to return before creating our image nodes._



## Importing external files

To import the `galleryData.js` and `apple-tv.css` files into our component, we will use the Framework component's `.config()` method. This method preloads any JavaScript or CSS files passed to it in the `includes` array.

    FamousFramework.scene('module name', {
    
       //... behaviors, events, state, tree not shown ../
       
     }).config({
         includes: [
             'galleryData.js',
             'apple-tv.css'
         ]
    });

Note the syntax above. The `galleryData.js` and `apple-tv.css` files are loaded before our component, so the `imageData` array is now accessible to our application and the CSS styles are applied.

Within your state object replace the empty `srcs` array with a reference to image data:

	 states: {
		      rotationValue: 0,  
		      srcs: imageData, // add your image data array   
		      contextSize: 500, 
		      positionZ:[]        
        } 



## Los im&aacute;genes

Now it's time to include our Spanish images! To do this, we will use the `content` behavior to insert the images as HTML into our `.gallery-item`: 
 
Add the following parameter to the `.gallery-item` in your behaviors object:
    
    '.gallery-item': {
         
         //.. other behaviors not shown ..//   
        
        'content': function($index, srcs){
             return  `<img src="${ srcs[$index] }" style="height:100px;width:100px"/>`
        }
     
     }

In the spirit of building futuristic UI's, the framework supports modules written in ES6, so above we can use ES6 style string interpolation to embed our image paths. ( Read more about [template strings here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/template_strings) ). But, don't worry ES5 loyalists! You can also use plain old string concatenation for this as well i.e. `'<img src="'+srcs[$index]+'" style="height:100px;width:100px"/>'`.
	
If you save and refresh your project, it should now look similar to the screenshot below:

![images](addimages.png)


Before moving on to the next step, your `apple-tv.js` file should look like the code below:

    FamousFramework.component('my-name:apple-tv', {
            behaviors: {
                '#root': {
                    'style': {
                        'perspective': '1000px',
                    }
                },
                '#rotator-node': {
                    'size': function(contextSize){ 
                        return [contextSize, contextSize]
                    },         
                    'align': [0.5,0.5],          
                    'mount-point':[0.5,0.5],
                    'origin':[0.5,0.5]     
                    'style': {
                        'background-color': 'red',
                    },
                    'rotation': function(rotationValue){ 
                        return [-Math.PI/2.1, 0, rotationValue] 
                    }
                },
                '.gallery-item':{
                    'size': [100,100],  
                    ,
                    '$repeat':function(srcs){
                        return srcs   
                    },
                    'position-x': function($index, contextSize){ 
                        return Math.random()* contextSize
                    },
                    'position-y':function($index, contextSize){ 
                        return Math.random()* contextSize
                    },
                    'position-z': function($index, positionZ){ 
                        return positionZ[$index] 
                    },
                    'rotation': [Math.PI/2,0,0], 
                    'content': function($index, srcs){
                         return  `<img src="${ srcs[$index] }" style="height:100px;width:100px"/>`
                    }
                }
            },          
            events: {},              
            states: {
              rotationValue: 0,    
              srcs: imageData,    
              contextSize: 500,   
              positionZ:[]       
            },              
            tree: 'apple-tv.html'  
    }).config({
         includes: [
             'galleryData.js'
         ]
    });

Note how we removed the styles on the `'.gallery-items'`, we will eventually do the same for the `#rotator` node.


<span class="cta">
[Up next: Positioning images &raquo;](./positioning-images.html)
</span>
