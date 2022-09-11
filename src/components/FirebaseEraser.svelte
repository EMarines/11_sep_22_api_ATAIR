<script>
   import { db } from "../firebase";

   let binnacle=[];
   let dataToFind;
   let colectionToFind;
   let contactBinnacle=[];

   db.collection("binnacles").onSnapshot((querySnapshot) => {
      let docs = [];
      querySnapshot.forEach((doc) => {
         docs.push({ ...doc.data(), id: doc.id });
      });
      binnacle = [...docs];          
   });
   
   function erraseBinnacle() {  
      // console.log("estas en erraseB", binnacle.length)                 
      let toErrase=[];
      binnacle.forEach(item => {
         try {                 
            toErrase = item.comment.indexOf(dataToFind) 
         } catch (error) {
            console.log(error, binnacle.date)
         }

         if(toErrase >= 0){
            let binnacleMessage = item;
            let id = binnacleMessage.id
            contactBinnacle.push(binnacleMessage)        
         }
   })  
   console.log(contactBinnacle.length)        
      let confirmar = confirm("Seguro que deseas borrar", contactBinnacle.length)
      if(confirmar == true){
         borrar(contactBinnacle)
         } else {
            alert("Cancelaste el borrado")
         }      
   };
   
   function borrar(){
      contactBinnacle.forEach(item =>{
      console.log(item.id)
      db.collection(colectionToFind).doc(item.id).delete();
   })
   alert("terminó borro los datos de fiebase")
   }
</script>

<form>

   <input type="text" placeholder="Dato a buscar para borrar"  bind:value={dataToFind}>
   <input type="text" placeholder="Colección de busqueda"  bind:value={colectionToFind}>
   <button on:click={erraseBinnacle}>borrar</button>

</form>