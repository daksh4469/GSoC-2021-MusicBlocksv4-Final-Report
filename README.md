# MusicBlocksv4 Menu Framework and ArtBoard Connections
### *Google Summer of Code, 2021*
#### *Author: Daksh Doshi ([daksh4469](https://github.com/daksh4469))*

![sugarlabs](https://user-images.githubusercontent.com/60084414/130358303-574093a9-7889-43e3-898a-9f15b32a62a8.png)

### sugarlabs/[musicblocks](https://github.com/sugarlabs/musicblocks-v4) 

## Project Details
- *Project Title: [MusicBlocksv4 Menu Framework and Connections with Artboard](https://summerofcode.withgoogle.com/dashboard/project/6691029673050112/overview/)*<br/>
- *Organization: [Sugarlabs](https://github.com/sugarlabs/)*<br/>
- *Mentors : [Anindya Kundu](https://github.com/meganindya)*<br/>
          &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;*[Walter Bender](https://github.com/walterbender)*<br />
          &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;*[Devin Ulibarri](https://github.com/pikurasa)*<br />
          &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;*[Peace Ojemeh](https://github.com/perriefidelis)*<br/>
          &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;

## Abstract
MusicBlocks is going through a complete overhaul, allowing us to work on a new, improved version with the latest tech stacks and enhanced performance. It has been a fantastic experience participating in Google Summer of Code 2021 with Sugar Labs. I am very grateful to Sugar Labs for guiding me throughout the project and giving me the opportunity to work alongside an amazing peer group.

The Toolbar in MusicBlocksv3 needed a refactor in terms of both design and the internal implementation. The functionalities in the toolbar needed to be better organized in a way that would enhance the User Experience and provide the most suitable accessibility to them. The latest version of MusicBlocks would have a more maintainable Menu catering to all the issues in the previous version with a new implementation.

## Deciding the Project Architecture
The first step of the project was to explore various architectures that would be the best fit for our tech stack involving React and Typescript. After various discussions with mentors, MVVM(Model-View-ViewModel) Architecture was found to be the perfect fit for our project meeting all our requirements. Thereafter, the project structure was decided to be consisting of various components like Menus, Palettes, and Block Framework with the Monitor connecting all of them. 

<p align="center">
  <img src="https://user-images.githubusercontent.com/60084414/130359064-aa8f81f2-81c8-4f43-aa55-f6b40214bcbb.png" />
</p>

## Segregation of Functionalities
The first step of this project was to decide upon the necessary functionalities to be kept in the new version based on user experiences. Thus, after discussion with mentors and fellow GSoC students, we decided to get rid of the “Modes” in MusicBlocks which were present in Basic and Advanced forms in version 3. Instead, we decided to go with the approach of “High-Shelf” and “Low-Shelf” organization of the Menu dock. High Shelf category would contain the features which a user does not access generally or more than once usually. An example of this would be the “Language Selection” feature. Low Shelf category would contain the feature that a user needs in hand regularly while building a project in Music Blocks. 

<p align="center">
  <img src="https://user-images.githubusercontent.com/60084414/130360559-7b5de66a-49b1-4c3a-a7e8-c4889cb3a5d4.png" />
</p>

## Designing the new UI
Using Figma as a prototyping tool, I designed a prototype framework keeping these requirements in mind. These mockups can be found [here](https://www.figma.com/file/Hsij6kzwr5WZiSQl9mP28v/Menu-Dock-MBv4?node-id=0%3A1).  This is a result of numerous discussions and brainstorming with mentors to achieve a clean design with optimal compactness.

<p align="center">
  <img src="https://user-images.githubusercontent.com/60084414/130359580-fd09ee4b-e17e-4817-99e1-ddfd67965397.png" />
  <img src="https://user-images.githubusercontent.com/60084414/130359694-2f33ab4a-4ef2-4255-b3bf-d1beccbea00d.png" />

</p>

## Implementing the Menu

The Model Component of the Menu Framework is defined in the `Menu.ts` file, the View-Model Component in the `Menu.ts` file in the components directory and the View Component is defined in the `Menu.tsx` and `Checkbox.tsx` files inside the `views/menu` directory. The View-Model maintains the state and the required methods according to the Model and serves them to the View Component. In simpler terms, the View-Model components acts as a link between the View and the Model Component. The directory structure regarding the Menu Framework Implementation is as follows:

<p align="center">
  <img src="https://user-images.githubusercontent.com/60084414/130360837-9489d6f3-2c42-4adf-a164-d29defd5081d.png">
</p>

The complete documentation of the Menu Framework can be found [here](https://github.com/sugarlabs/musicblocks-v4/tree/milestone-2/src/components/menu).

<hr/>

### Auto-Hide Menu
The Menu now auto-hides itself when the mouse cursor is not in its near vicinity. This is implemented using an overlay which tracks the movement of mouse pointer and updates the states `autoHide` and `autoHideTemp` of the Menu whenever the cursor enters or leaves through the boundary of overlay. This feature is developed to increase the working space of the user substantially. A user can this way, access the Menu Dock only when he feels the need to do so and does not have to always get his workspace blocked by the Menu.

<p align="center">
<img src="https://user-images.githubusercontent.com/60084414/130365466-68638db5-ff3a-4086-a151-2991f1596011.gif" />
</p>

### Context Configurations 
The Global Settings of MusicBlocksv4 are stored in the form an object in the Context API and is implemented using the `useContext()` hook of the React Framework. The structure(interface) of this `config` object is defined as:

``` js script
interface IConfig {
    theme: 'light' | 'dark';
    language: string;
    horizontalScroll: boolean;
    turtleWrap: boolean;
    blockSize: number;
    masterVolume: number;
}
```

The Config object is initialized as a state in the `App.tsx` file using the `useState()` hook. It is then passed on to the children components of the application using the `Provider` React Component that is provided for every defined Context. The attributes of this API are then utilized in the children using the `Consumer` React Component. The values of these attributes can be updated through the `Settings` and `Music Settings` tabs in the Menu Dock. The Config object of the Context API can be utilised in any child component in the following manner:

``` js script
const { config, setConfig } = useContext(ContextConfig);
```

<hr/>

### Turtle Wrap and Clean Functionality
While a project is running, the turtle can possibly go out of the window parameters. Hence, a `Turtle Wrap` can be enabled to handle this case so that, whenever a turtle goes out of the window parameters, its coordinates get reset to the side opposite to the current side from where it has gone out of bounds. This can enable a user to view the complete artwork of the turtle by limiting its area to the window size. We have maintained a `turtleWrap` parameter in the Config object of the Context API that maintains the state of enabling the Turtle Wrap. 

The Wrap functionality is implemented in the `draw()` method in the `ArtboardSketch.tsx` file of the ArtBoard Component, which maintains the Artwork of an individual turtle, and this is done by comparing the boolean `config.turtleWrap` at every step of sketching the artwork of every individual turtle.

#### When Turtle Wrap is Off:

![wrapOff](https://user-images.githubusercontent.com/60084414/130364007-71d5b0c5-1c70-45c7-96e4-eeeb135d714b.gif)

#### When the Turtle Wrap in On:

![wrapOn](https://user-images.githubusercontent.com/60084414/130364094-bb9d0ce5-3d90-4086-bac9-6374f117bcda.gif)


The Clean functionality has also been implemented so that the artwork of each and every turtle can be cleaned through the Menu Dock. This is done by defining a method in `Artboard.tsx` to call the `clean()` methods in the `ArtboardSketch.tsx` file for every turtle in the Artboard. The artwork of every turtle is cleaned by a utility function of the `p5` library which provides us with the option to clean the sketch by calling `sketch.clear()` for every artwork on the artboard.

![clean](https://user-images.githubusercontent.com/60084414/130364100-fbafcf75-620c-41a6-8a91-31208c6cd21b.gif)

<hr/>

## Connecting Menu and ArtBoard through the Monitor

The `Monitor.ts` file contains the classes `Menu` and `Artboard` based off of the interfaces `IMenu` and `IArtboard` and these classes are intantiated in the default constructor of the `Monitor` class wherein, the menu and artboard are present as member variables under the `private` access modifier(as `_menu` and `_artboard` respectively). Hence, the menu and artboard in the entire application can only be accessed by their repsective `getter` functions defined in the `Monitor` class. Now, various methods to update the Config State of the Context API from the children components are registered in the Monitor from various files. For example: The method to update the state of Turtle Wrap from the Menu dock needs to be registerd in the Menu through the Monitor as follows:

``` js script
registerUpdateWrap(updateTurtleWrap: (isWrapOn: boolean) => void): void {
      this._menu.updateTurtleWrap = updateTurtleWrap;
}
```
In this way, two methods for the same functionality are registered from two different components which need to be connected. For Example: The `cleanArtwork()` method that is called from the Menu View is registered from the Menu Component and the `clean()` method is registered from the Artboard component and in thus, these two components are communicate with each other for the Clean functionality by the following:

``` js script
registerClean(): void {
      this._menu.cleanArtwork = this._artboard.clean;
}
```

## Contributions to the Project:

| Pull Request | Description |
|----|----|
| [#72: Menu Framework](https://github.com/sugarlabs/musicblocks-v4/pull/72) | <ul> <li> Built the initial Menu View and View Model Components following the MVVM Architecture. </li> <li> Add the data required by the Menu Framework in the Monitor to be fetched on initial render of the Menu View. </li> <li>Implemented the Auto-Hide feature for the Menu Dock</li><li>Created an initial state of the Configuration object of the Context API.</li></ul> | 
| [#77: App Configurations in Menu Framework](https://github.com/sugarlabs/musicblocks-v4/pull/77) | <ul><li> Build the Project Settings Submenu accesible through the Menu Dock with the options to Save, Load, Merge, and create a new project. </li> <li> Build the Global Settings Submenu accesible to the Menu Dock with options to enable Turtle Wrap, Horizontal Scroll, and set the desired Block Size and Language. </li> <li> Establish the connection between Monitor and the Menu Framework and register the methods to update Global Configuration in the Context API. </li> </ul> |
| [#82: Music Configurations in Menu Framework](https://github.com/sugarlabs/musicblocks-v4/pull/82) | <ul> <li> Built the Music Setting Submenu accesible through the Menu Dock with the options to set the pitch, temperament, master volume and view status of the running project. </li> <li> Built custom Checkbox component with a input in the form of checkbox alongwith a custom title. </li> <li> Register the methods to update the applicable parameters of the Configuration through the Monitor </li> <li> Resolved some bugs related to synchronization of the opening of all the submenus and visibility of the tooltips for every list item in the Menu Dock. </li> </ul> |
| [#88: Connect Menu and Artboard framework with Monitor](https://github.com/sugarlabs/musicblocks-v4/pull/88) | <ul> <li> Refactor code of the previous work and adjust positioning of the Menu Dock. </li> Added the Menu Dock functionalities related methods to the Monitor that are to be called from the Menu Framework and update the Menu View to call these methods. <li> Added the Turtle Wrap functionality in the ArtBoard Sketch dependent on the Configuration state. </li> <li> Implmented the Clean Artwork functionality in the ArtBoard framework and connect the Menu and ArtBoard through the Monitor.  </li> <li> Refactor the code with removal of various debug logs and adding suggestive comments throughtout the codebase. </li> </ul> |
| [#93: Menu Documentation](https://github.com/sugarlabs/musicblocks-v4/pull/93) | <ul> <li> Added the Menu Framework Documentation. </li> </ul> |

## Acknowledgements

On a final note, I'd like to thank all my mentors: [Anindya Kundu](https://github.com/meganindya), [Walter Bender](https://github.com/walterbender), [Devin Ulibarri](https://github.com/pikurasa), and [Peace Ojemeh](https://github.com/perriefidelis) for guiding me through the thought process of building this project and providing their valuable feedback in every stage of the process. I learnt a lot about writing quality and maintainable code through this project. I am also grateful to all my peers: [Joykirat Singh](https://github.com/joykirat18), [Kumar Saurabh Raj](https://github.com/ksraj123), and [Chandan Prakash](https://github.com/b18050) as this project was not a stand-alone one and needed cooperation and valuable help from everyone. Lastly, I'd like to acknowledge Google for conducting such an amazing program.

Looking forward to contributing more, in any way I can.
