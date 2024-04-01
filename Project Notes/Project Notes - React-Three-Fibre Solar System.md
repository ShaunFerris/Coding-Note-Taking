#projects 

This project is a small toy project to fill time and keep my skills sharp while I study and apply for jobs actively. Feedback from a recent interview has made me doubt homeHub as a resume app because even though it is relatively complex and has a testing suite and other advanced features, it does not impress enough to escape the stigma of "todo apps".

Also taking the opportunity to get more experience using vite rather than next to scaffold the project.

## Dependancies and new tech
Most of the dependancies I plan to use in this project are ones I have used a lot, like typescript, shadcnUI etc, the spicy new additions that I do not at all know how to use yet are react-three-fiber (RTF going forward) and drei, two libraries for using the Three.js webGL library within a react project. 

Some relevant resources are found in these docs pages:
- [Three.js docs](https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene)
- [React-three-fiber docs](https://docs.pmnd.rs/react-three-fiber/getting-started/introduction)
- [Drei docs](https://drei.pmnd.rs/?path=/docs/controls-orbitcontrols--docs)
- [Live preview of a demo of what I am going for that someone else built](https://codesandbox.io/p/sandbox/animated-solarsystem-with-react-three-fiber-2-1-w821r?file=%2Fpackage.json)

## Initial setup
Nothing new or interesting is expected here, scaffolding the project with vite and adding dependancies, then removing boilerplate.

After that will set up an initial "scene" using RTF and start experimenting with adding objects before making a real start on the solar system.

Something I unexpectedly ran into while getting this up and running was that I actually needed to install tailwind this time. I have gotten so used to spinning up new projects with next that I completely forgot that it is normal to set it up yourself lmao. Useful docs are here: [tailwind setup docs](https://tailwindcss.com/docs/guides/vite)

After getting rid of all the boilerplate in App.tsx and the css files, I setup a canvas to host and render the 3D scene, and styled it to take the full screen. The canvas object is the the component where you define the scene and pass child elements to be rendered in the scene. It can also take args to pass down to the renderer, camera and lighting in the scene. Docs for the canvas element are [here](https://docs.pmnd.rs/react-three-fiber/api/canvas)

## Layout of the app
Ultimately I want the app to have a 3d rendered scene of a solar system type style, with spheres orbiting around a central point. I would also like to have some amount of control exposed to the user to add/remove orbiting objects, and possibly atler orbital properties of the objects. 

Before getting into the nitty gritty of animating such a scene and exposing controls to the user I need to get the basic layout sorted. I want a tranparent header and footer bar with basics like a title in the header and a copyright notice in the footer. I want some kind of control panel that can be shown or hidden for user control, and I want the canvas to take up all available space with the header/footer/control panel overlayed on top. 

I set up the bones of this using tailwind classes by setting the div that wraps the App component to relative display, then setting all of its children (AnimationCanvas, Header, Footer etc) to absolute and pinning them in the proper places, like this:
```tsx
import { Canvas } from "@react-three/fiber";
import { OrbitControls } from "@react-three/drei";
import "./index.css";
import Footer from "./components/footer";
import Header from "./components/header";

export default function App() {
  return (
    <div className="relative h-screen">
      <Header />
      <AnimationScene />
      <Footer />
    </div>
  );
}

function AnimationScene() {
  return (
    <Canvas className="absolute top-0 left-0 w-full h-full z-0">
      <Sol />
      <OrbitControls />
      <MainLighting />
    </Canvas>
  );
}

function Sol() {
  return (
    <mesh>
      <sphereGeometry args={[2.5, 32, 32]} />
      {/* meshStandardMaterial is a lit mesh, its black unless you add a light */}
      <meshStandardMaterial color="#E1DC59" />
    </mesh>
  );
}

function MainLighting() {
  return (
    <>
      <ambientLight />
      <pointLight position={[0, 0, 0]} />
    </>
  );
}
```