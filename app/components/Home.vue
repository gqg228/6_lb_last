<template >
    <Page class="pogoda">
      <StackLayout>
        <Button text = "Where do you live?" @tap='chose()'/>
           <StackLayout class='qwerty' orientation="vertical">
             <AbsoluteLayout>
             <Label text = "City" left="10" top="10" width="100" height="100"/>
             <Label class='text1' left="120" top="10"  :text='listOfItems[this.selectedItem]'/>
             </AbsoluteLayout>
           <AbsoluteLayout>
               <Label text = "Season" left="10" top="10" width="100" height="100"/>
                <Label class='text1' left="120" top="10" :text='weather.fact.season'/>
             </AbsoluteLayout>

              <AbsoluteLayout>
                <Label text = "Temp" left="10" top="10" width="100" height="100"/>
                <Label class='text1' left="120" top="10" :text='weather.fact.temp'/>
              </AbsoluteLayout>
                <AbsoluteLayout>
                   <Label text = "Wind" left="10" top="10" width="100" height="100"/>
                   <Label class='text1' left="120" top="10" :text='weather.fact.wind_speed'/>
              </AbsoluteLayout>
                 
           </StackLayout>
         
      </StackLayout>
    </Page>
</template>

<script >
import { Http } from '@nativescript/core'
import * as ApplicationSettings from "@nativescript/core/application-settings";
  export default {
    data() {
      return {
        listOfItems: [
           
         
          { title: "Khanty-Mansiysk",
            toString: () => {
              return 'Khanty-Mansiysk';
            },
            latitude: 61.004,
            longitude: 69.001
          },
          { title: "Tumen",
            toString: () => {
              return 'Tumen';
            },
            latitude: 61.004,
            longitude: 69.001
          },
          { title: "New York",
            toString: () => {
              return 'New York';
            },
            latitude: 40.714606, 
            longitude: -74.002800
          },
          { title: "Surgut",
            toString: () => {
              return 'Surgut';
            },
            latitude: 61.254035, 
            longitude: 73.396230
          },
          { title: "Kipr",
            toString: () => {
              return 'Kipr';
            },
            latitude: 35.169998,
            longitude:  33.359468
          },
          { title: "Moscow",
            toString: () => {
              return 'Moskow';
            },
            latitude: 55.753220,
            longitude:  37.622513
          },
          { title: "Nursultan",
            toString: () => {
              return 'Nursultan';
            },
            latitude: 51.128038, 
            longitude:  71.430307
          },
          
        ],
        selectedItem: 1,
        weather: {
            fact: {
                temp : 0,
                wind_speed : 0,
                season: 'summer,'
            }
        },
        imagePath: '',
        cities:['Khanty-Mansiysk', 'Tumen','New York','Surgut','Kipr','Moscow','Nursultan']
      }
    },
    mounted(){
      if(ApplicationSettings.getString('weather')){
        this.weather.fact=JSON.parse(ApplicationSettings.getString('weather'));
       
      }
      if(ApplicationSettings.getString('chose')){
        this.selectedItem=JSON.parse(ApplicationSettings.getString('chose'));
       
            }
            else{
                  this.chose()
      }
     
    },
        methods:{
      check(){
      Http.request({
        url: 'https://api.weather.yandex.ru/v2/forecast?limit=1'+'&lat=' + 
        String(this.listOfItems[this.selectedItem].latitude) + '&lon=' +
        String(this.listOfItems[this.selectedItem].longitude),
        method: "GET",
        headers: {"X-Yandex-API-Key": "2774d3ce-4acb-4d49-8852-4495f84d391b"},
        })
        .then(
        (response) => {
        this.weather = response.content.toJSON();
        
        
      });
      },
      hz(){
        alert({
          title: "Ваш заголовок",
           message: "Ваше сообщение",
        okButtonText: "Ваш текст кнопки OK"
        }).then(() => {
         console.log("Диалоговое окно закрыто");
        });
      },
      chose(){
        action("Ur choice", "exit", this.cities)
        .then(result => {
           this.selectedItem = this.cities.indexOf(result);
           ApplicationSettings.setString('chose', JSON.stringify(this.selectedItem));        
           this.check();
           
        });
      }
    }
  }
</script>

<style>
.city{
  font-size: 30;
  text-align: center;
  width: 100%;
  background-color: #ffffff;
}
.pogoda{
    background-color: #ffffff;
}



Actionbar{
  background-color: #fece2d;
  color : #fd3c3f;
}
.qwerty{
  font-size: 22;
  padding: 20px;
  padding-bottom: 270px;
  background-color: #ffffff;
  border-radius: 10%;
}
</style>