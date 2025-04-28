# Framer Motion: A Complete Guide for Beginners

## Table of Contents
1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Basic Concepts](#basic-concepts)
4. [Basic Animations](#basic-animations)
5. [Variants](#variants)
6. [Gestures](#gestures)
7. [Page Transitions](#page-transitions)
8. [Scroll Animations](#scroll-animations)
9. [Advanced Techniques](#advanced-techniques)
10. [Performance Tips](#performance-tips)
11. [Project Examples](#project-examples)
12. [Resources](#resources)

## Introduction

Framer Motion is a production-ready animation library for React that makes creating animations and interactive UI elements simple and intuitive. It provides a declarative API that allows you to create complex animations with minimal code.

Key features of Framer Motion:
- Simple declarative animations
- Gesture recognition (drag, tap, hover)
- Layout animations
- Variants for orchestrating animations
- Server-side rendering support
- Exit animations

## Installation

To get started with Framer Motion, you'll need to install it in your React project:

```bash
# Using npm
npm install framer-motion

# Using yarn
yarn add framer-motion

# Using pnpm
pnpm add framer-motion
```

## Basic Concepts

### The `motion` Component

Framer Motion's core concept is the `motion` component. It's an animated version of HTML and SVG elements. For example, `motion.div` is an animated `div`.

```jsx
import { motion } from "framer-motion";

// Basic usage
function App() {
  return (
    <motion.div
      animate={{ x: 100 }}
      transition={{ duration: 2 }}
    >
      I'll move 100px to the right
    </motion.div>
  );
}
```

### Animation Properties

Framer Motion supports animating a wide range of properties:
- Position (`x`, `y`, `z`)
- Dimensions (`width`, `height`)
- Scale (`scale`, `scaleX`, `scaleY`)
- Rotation (`rotate`, `rotateX`, `rotateY`, `rotateZ`)
- Opacity
- Color
- And many more CSS properties

### Transitions

The `transition` prop defines how properties animate:

```jsx
<motion.div
  animate={{ x: 100 }}
  transition={{
    duration: 2,        // Animation takes 2 seconds
    delay: 0.5,         // Starts after 0.5 seconds
    ease: "easeInOut",  // Easing function
    type: "spring",     // Animation type
    stiffness: 100,     // Spring stiffness (for spring animations)
    damping: 10         // Spring damping (for spring animations)
  }}
>
  Animated element
</motion.div>
```

## Basic Animations

### Simple Animation

```jsx
import { motion } from "framer-motion";

function SimpleAnimation() {
  return (
    <motion.div
      initial={{ opacity: 0, scale: 0.5 }}
      animate={{ opacity: 1, scale: 1 }}
      transition={{ duration: 0.5 }}
    >
      I will fade in and scale up
    </motion.div>
  );
}
```

### Animating on Hover

```jsx
function HoverAnimation() {
  return (
    <motion.button
      initial={{ backgroundColor: "#3498db" }}
      whileHover={{ 
        scale: 1.1,
        backgroundColor: "#2980b9",
        boxShadow: "0px 5px 10px rgba(0, 0, 0, 0.2)"
      }}
      transition={{ duration: 0.2 }}
    >
      Hover me
    </motion.button>
  );
}
```

### Animating on Tap/Click

```jsx
function TapAnimation() {
  return (
    <motion.button
      whileTap={{ scale: 0.9 }}
      transition={{ type: "spring", stiffness: 400, damping: 17 }}
    >
      Click me
    </motion.button>
  );
}
```

### Keyframes Animation

You can create keyframe animations by passing arrays to animated properties:

```jsx
function KeyframesAnimation() {
  return (
    <motion.div
      animate={{
        x: [0, 100, 0],
        backgroundColor: ["#ff0000", "#00ff00", "#0000ff", "#ff0000"],
        scale: [1, 1.5, 1]
      }}
      transition={{
        duration: 2,
        ease: "easeInOut",
        times: [0, 0.5, 1], // Control timing of keyframes
        repeat: Infinity,    // Repeat infinitely
        repeatType: "loop"   // "loop" or "reverse" or "mirror"
      }}
      style={{ width: 100, height: 100 }}
    />
  );
}
```

## Variants

Variants allow you to define animation states and orchestrate animations across multiple components.

```jsx
const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: {
      when: "beforeChildren", // Wait for parent to finish before animating children
      staggerChildren: 0.1    // Stagger children animations by 0.1 seconds
    }
  }
};

const itemVariants = {
  hidden: { y: 20, opacity: 0 },
  visible: {
    y: 0,
    opacity: 1
  }
};

function VariantsExample() {
  return (
    <motion.ul
      variants={containerVariants}
      initial="hidden"
      animate="visible"
    >
      {[1, 2, 3, 4].map(i => (
        <motion.li key={i} variants={itemVariants}>
          Item {i}
        </motion.li>
      ))}
    </motion.ul>
  );
}
```

## Gestures

### Drag Gesture

```jsx
function DragExample() {
  return (
    <motion.div
      drag
      dragConstraints={{ left: -100, right: 100, top: -100, bottom: 100 }}
      dragElastic={0.2}
      whileDrag={{ scale: 1.1 }}
      style={{
        width: 100,
        height: 100,
        backgroundColor: "red",
        borderRadius: 10
      }}
    />
  );
}
```

### Drag with `useDragControls`

```jsx
import { motion, useDragControls } from "framer-motion";

function DragControlsExample() {
  const dragControls = useDragControls();

  function startDrag(e) {
    dragControls.start(e, { snapToCursor: true });
  }

  return (
    <>
      <div onPointerDown={startDrag}>Drag from here</div>
      <motion.div
        drag="x"
        dragControls={dragControls}
        style={{
          width: 100,
          height: 100,
          backgroundColor: "blue",
          borderRadius: 10
        }}
      />
    </>
  );
}
```

## Page Transitions

For page transitions in a React application, you can use Framer Motion's `AnimatePresence` to animate components when they're added or removed from the React tree.

```jsx
import { motion, AnimatePresence } from "framer-motion";
import { Routes, Route, useLocation } from "react-router-dom";

function App() {
  const location = useLocation();
  
  return (
    <AnimatePresence mode="wait">
      <Routes location={location} key={location.pathname}>
        <Route path="/" element={<HomePage />} />
        <Route path="/about" element={<AboutPage />} />
        <Route path="/contact" element={<ContactPage />} />
      </Routes>
    </AnimatePresence>
  );
}

const pageTransition = {
  initial: { opacity: 0, x: -200 },
  animate: { 
    opacity: 1, 
    x: 0,
    transition: { duration: 0.5 }
  },
  exit: { 
    opacity: 0, 
    x: 200,
    transition: { duration: 0.5 }
  }
};

function HomePage() {
  return (
    <motion.div
      variants={pageTransition}
      initial="initial"
      animate="animate"
      exit="exit"
    >
      <h1>Home Page</h1>
    </motion.div>
  );
}

// Similar for AboutPage and ContactPage
```

## Scroll Animations

### Basic Scroll-Triggered Animation

```jsx
import { motion } from "framer-motion";

function ScrollAnimation() {
  return (
    <motion.div
      initial={{ opacity: 0, y: 50 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, amount: 0.8 }}
      transition={{ duration: 0.5 }}
    >
      This will animate when scrolled into view
    </motion.div>
  );
}
```

### Scroll-Linked Animation

For more advanced scroll animations, you can use the `useScroll` hook:

```jsx
import { motion, useScroll, useTransform } from "framer-motion";

function ParallaxEffect() {
  const { scrollYProgress } = useScroll();
  const y = useTransform(scrollYProgress, [0, 1], [0, 300]);
  const opacity = useTransform(scrollYProgress, [0, 0.5, 1], [1, 0.5, 0]);
  
  return (
    <motion.div
      style={{
        y,
        opacity,
        height: 500,
        backgroundColor: "purple"
      }}
    >
      Parallax Element
    </motion.div>
  );
}
```

## Advanced Techniques

### Layout Animation

Layout animations automatically animate elements when their layout changes:

```jsx
import { useState } from "react";
import { motion, AnimatePresence } from "framer-motion";

function LayoutAnimationExample() {
  const [items, setItems] = useState([0, 1, 2, 3]);

  const removeItem = (itemIndex) => {
    setItems(items.filter((_, i) => i !== itemIndex));
  };

  return (
    <div>
      <button onClick={() => setItems([...items, items.length])}>
        Add Item
      </button>
      
      <div style={{ display: "flex", flexWrap: "wrap" }}>
        <AnimatePresence>
          {items.map((item) => (
            <motion.div
              key={item}
              layout
              initial={{ opacity: 0, scale: 0.8 }}
              animate={{ opacity: 1, scale: 1 }}
              exit={{ opacity: 0, scale: 0.8 }}
              onClick={() => removeItem(item)}
              style={{
                width: 100,
                height: 100,
                margin: 10,
                backgroundColor: "teal",
                borderRadius: 10,
                display: "flex",
                justifyContent: "center",
                alignItems: "center",
                cursor: "pointer"
              }}
            >
              {item}
            </motion.div>
          ))}
        </AnimatePresence>
      </div>
    </div>
  );
}
```

### Shared Layout Animation

`layoutId` allows elements to transition between different components:

```jsx
import { useState } from "react";
import { motion } from "framer-motion";

function SharedLayoutExample() {
  const [selectedId, setSelectedId] = useState(null);

  const items = [
    { id: 1, title: "Item 1", color: "#ff0055" },
    { id: 2, title: "Item 2", color: "#0099ff" },
    { id: 3, title: "Item 3", color: "#22cc88" }
  ];

  return (
    <div>
      <div style={{ display: "flex", gap: 20 }}>
        {items.map((item) => (
          <motion.div
            key={item.id}
            layoutId={`card-${item.id}`}
            onClick={() => setSelectedId(item.id)}
            style={{
              width: 100,
              height: 100,
              backgroundColor: item.color,
              borderRadius: 10,
              cursor: "pointer"
            }}
          />
        ))}
      </div>

      <AnimatePresence>
        {selectedId && (
          <motion.div
            layoutId={`card-${selectedId}`}
            style={{
              position: "fixed",
              top: "50%",
              left: "50%",
              transform: "translate(-50%, -50%)",
              width: 300,
              height: 300,
              backgroundColor: items.find(item => item.id === selectedId).color,
              borderRadius: 20,
              zIndex: 10
            }}
            onClick={() => setSelectedId(null)}
          >
            <h2>{items.find(item => item.id === selectedId).title}</h2>
            <p>Expanded content here</p>
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  );
}
```

### Motion Values

`useMotionValue` and `useTransform` allow for fine-grained control:

```jsx
import { motion, useMotionValue, useTransform } from "framer-motion";

function MotionValueExample() {
  const x = useMotionValue(0);
  const opacity = useTransform(x, [-200, 0, 200], [0, 1, 0]);
  const scale = useTransform(x, [-200, 0, 200], [0.5, 1, 1.5]);
  
  return (
    <motion.div
      drag="x"
      style={{ 
        x,
        opacity,
        scale,
        width: 100,
        height: 100,
        backgroundColor: "blue",
        borderRadius: 10
      }}
    />
  );
}
```

## Performance Tips

1. **Use `layout` Sparingly**: Layout animations can be expensive. Only use on elements that need it.

2. **Use the `at` Prop for CSS Transform Properties**:
   ```jsx
   // More performant
   <motion.div animate={{ x: 100 }} />
   
   // Less performant
   <motion.div animate={{ marginLeft: 100 }} />
   ```

3. **Use Hardware Acceleration with Caution**:
   ```jsx
   <motion.div style={{ willChange: "transform" }} />
   ```

4. **Use Exit Before Enter with `AnimatePresence`**:
   ```jsx
   <AnimatePresence mode="wait">
     {/* Your components */}
   </AnimatePresence>
   ```

5. **Memoize Components with `React.memo`**:
   ```jsx
   const MemoizedComponent = React.memo(MyAnimatedComponent);
   ```

## Project Examples

Here are a few simple projects to help you practice Framer Motion:

### 1. Animated Navigation Menu

```jsx
import { useState } from "react";
import { motion } from "framer-motion";

function AnimatedMenu() {
  const [isOpen, setIsOpen] = useState(false);
  
  const menuVariants = {
    closed: {
      height: 0,
      opacity: 0,
      transition: {
        staggerChildren: 0.05,
        staggerDirection: -1
      }
    },
    open: {
      height: "auto",
      opacity: 1,
      transition: {
        staggerChildren: 0.1,
        staggerDirection: 1
      }
    }
  };
  
  const itemVariants = {
    closed: { opacity: 0, y: -20 },
    open: { opacity: 1, y: 0 }
  };
  
  return (
    <div>
      <button onClick={() => setIsOpen(!isOpen)}>
        {isOpen ? "Close" : "Open"} Menu
      </button>
      
      <motion.ul
        variants={menuVariants}
        initial="closed"
        animate={isOpen ? "open" : "closed"}
        style={{ 
          listStyle: "none",
          overflow: "hidden",
          padding: 0
        }}
      >
        {["Home", "About", "Services", "Contact"].map((item) => (
          <motion.li
            key={item}
            variants={itemVariants}
            style={{
              padding: "10px 20px",
              backgroundColor: "#f5f5f5",
              margin: "5px 0",
              borderRadius: 5
            }}
          >
            {item}
          </motion.li>
        ))}
      </motion.ul>
    </div>
  );
}
```

### 2. Image Gallery with Animations

```jsx
import { useState } from "react";
import { motion, AnimatePresence } from "framer-motion";

function ImageGallery() {
  const images = [
    "/image1.jpg",
    "/image2.jpg",
    "/image3.jpg",
    "/image4.jpg",
  ];
  
  const [selectedImage, setSelectedImage] = useState(null);
  
  return (
    <div>
      <div style={{ 
        display: "grid", 
        gridTemplateColumns: "repeat(auto-fill, minmax(150px, 1fr))",
        gap: 10
      }}>
        {images.map((src, index) => (
          <motion.img
            key={src}
            src={src}
            layoutId={`image-${index}`}
            onClick={() => setSelectedImage(index)}
            whileHover={{ scale: 1.05 }}
            whileTap={{ scale: 0.95 }}
            style={{ 
              width: "100%", 
              height: 150, 
              objectFit: "cover",
              borderRadius: 10,
              cursor: "pointer"
            }}
          />
        ))}
      </div>
      
      <AnimatePresence>
        {selectedImage !== null && (
          <motion.div
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            style={{
              position: "fixed",
              top: 0,
              left: 0,
              right: 0,
              bottom: 0,
              backgroundColor: "rgba(0, 0, 0, 0.8)",
              display: "flex",
              justifyContent: "center",
              alignItems: "center",
              zIndex: 100
            }}
            onClick={() => setSelectedImage(null)}
          >
            <motion.img
              layoutId={`image-${selectedImage}`}
              src={images[selectedImage]}
              style={{
                maxWidth: "80%",
                maxHeight: "80%",
                borderRadius: 20
              }}
            />
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  );
}
```

### 3. Animated Todo Application

```jsx
import { useState } from "react";
import { motion, AnimatePresence } from "framer-motion";

function AnimatedTodo() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState("");
  
  const addTodo = (e) => {
    e.preventDefault();
    if (input.trim()) {
      setTodos([...todos, { id: Date.now(), text: input }]);
      setInput("");
    }
  };
  
  const removeTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };
  
  return (
    <div>
      <form onSubmit={addTodo}>
        <input
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="Add a todo"
        />
        <motion.button
          whileHover={{ scale: 1.05 }}
          whileTap={{ scale: 0.95 }}
          type="submit"
        >
          Add
        </motion.button>
      </form>
      
      <motion.ul style={{ listStyle: "none", padding: 0 }}>
        <AnimatePresence>
          {todos.map((todo) => (
            <motion.li
              key={todo.id}
              initial={{ opacity: 0, height: 0 }}
              animate={{ opacity: 1, height: "auto" }}
              exit={{ opacity: 0, height: 0 }}
              transition={{ type: "spring" }}
              style={{
                display: "flex",
                justifyContent: "space-between",
                alignItems: "center",
                padding: "10px 20px",
                margin: "10px 0",
                backgroundColor: "#f5f5f5",
                borderRadius: 5
              }}
            >
              <span>{todo.text}</span>
              <motion.button
                whileHover={{ scale: 1.1, color: "#ff0000" }}
                whileTap={{ scale: 0.9 }}
                onClick={() => removeTodo(todo.id)}
                style={{ 
                  background: "none", 
                  border: "none",
                  cursor: "pointer"
                }}
              >
                ×
              </motion.button>
            </motion.li>
          ))}
        </AnimatePresence>
      </motion.ul>
    </div>
  );
}
```

## Resources

### Official Resources
- [Framer Motion Documentation](https://www.framer.com/motion/)
- [Framer Motion API Reference](https://www.framer.com/motion/component/)
- [Framer Motion GitHub](https://github.com/framer/motion)

### Tutorials and Guides
- [Framer Motion Examples](https://www.framer.com/motion/examples/)
- [Framer Motion - The React Animation Library](https://www.youtube.com/watch?v=2V1WK-3HQNk) by Fireship
- [React Framer Motion Tutorial](https://www.youtube.com/watch?v=1vKiPwEYbyk) by Dev Ed

### Communities
- [Framer Discord](https://discord.framer.com)
- [Framer Forum](https://www.framer.com/community/)
- [StackOverflow Framer Motion Tag](https://stackoverflow.com/questions/tagged/framer-motion)

---

This guide should give you a solid foundation to start working with Framer Motion. As you become more comfortable, explore the official documentation for more advanced features and techniques. Happy animating!
