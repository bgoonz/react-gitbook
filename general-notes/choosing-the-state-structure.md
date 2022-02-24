# Choosing the State Structure

## Copy of Choosing the State Structure

[https://beta.reactjs.org/learn/choosing-the-state-structure](https://beta.reactjs.org/learn/choosing-the-state-structure)

Structuring state well can make a difference between a component that is pleasant to modify and debug, and one that is a constant source of bugs. Here are some tips you should consider when structuring state.

### Principles for structuring state

When you write a component that holds some state, you’ll have to make choices about how many state variables to use and what the shape of their data should be. While it’s possible to write correct programs even with a suboptimal state structure, there are a few principles that can guide you to make better choices:

1. **Group related state.** If you always update two or more state variables at the same time, consider merging them into a single state variable.
2. **Avoid contradictions in state.** When the state is structured in a way that several pieces of state may contradict and “disagree” with each other, you leave room for mistakes. Try to avoid this.
3. **Avoid redundant state.** If you can calculate some information from the component’s props or its existing state variables during rendering, you should not put that information into that component’s state.
4. **Avoid duplication in state.** When the same data is duplicated between multiple state variables, or within nested objects, it is difficult to keep them in sync. Reduce duplication when you can.
5. **Avoid deeply nested state.** Deeply hierarchical state is not very convenient to update. When possible, prefer to structure state in a flat way.

The goal behind these principles is to _make state easy to update without introducing mistakes_. Removing redundant and duplicate data from state helps ensure that all its pieces stay in sync. This is similar to how a database engineer might want to [“normalize” the database structure](https://docs.microsoft.com/en-us/office/troubleshoot/access/database-normalization-description) to reduce the chance of bugs. To paraphrase Albert Einstein, **“Make your state as simple as it can be—but no simpler.”**

Now let’s see how these principles apply in action.

### Group related state

You might sometimes be unsure between using a single or multiple state variables.

Should you do this?

Or this?

Technically, you can use either of these approaches. But **if some two state variables always change together, it might be a good idea to unify them into a single state variable**. Then you won’t forget to always keep them in sync, like in this example where moving the cursor updates both coordinates of the red dot:

import { useState } from 'react';

export default function MovingDot() {

const \[position, setPosition] = useState({

x: 0,

});

onPointerMove={e => {

x: e.clientX,

y: e.clientY

});

style=\{{

position: 'relative',

width: '100vw',

height: '100vh',

\}}>

position: 'absolute',

backgroundColor: 'red',

borderRadius: '50%',

transform: `translate(${position.x}px, ${position.y}px)`,

left: -10,

top: -10,

width: 20,

height: 20,

\}} />

Another case where you’ll group data into an object or an array is when you don’t know how many different pieces of state you’ll need. For example, it’s helpful when you have a form where the user can add custom fields.

If your state variable is an object, remember that [you can’t update only one field in it](https://beta.reactjs.org/learn/updating-objects-in-state) without explicitly copying the other fields. For example, you can’t do `setPosition({ x: 100 })` in the above example because it would not have the `y` property at all! Instead, if you wanted to set `x` alone, you would either do `setPosition({ ...position, x: 100 })`, or split them into two state variables and do `setX(100)`.

### Avoid contradictions in state

Here is a hotel feedback form with `isSending` and `isSent` state variables:

import { useState } from 'react';

export default function FeedbackForm() {

const \[text, setText] = useState('');

const \[isSending, setIsSending] = useState(false);

const \[isSent, setIsSent] = useState(false);

async function handleSubmit(e) {

e.preventDefault();

setIsSending(true);

await sendMessage(text);

setIsSending(false);

setIsSent(true);

if (isSent) {

return

## Thanks for feedback!

How was your stay at The Prancing Pony?

disabled={isSending}

value={text}

onChange={e => setText(e.target.value)}

\


disabled={isSending}

{isSending &&

Sending...

}

While this code works, it leaves the door open for “impossible” states. For example, if you forget to call `setIsSent` and `setIsSending` together, you may end up in a situation where both `isSending` and `isSent` are `true` at the same time. The more complex your component is, the harder it will be to understand what happened.

**Since `isSending` and `isSent` should never be `true` at the same time, it is better to replace them with one `status` state variable that may take one of **_**three**_** valid states:** `'typing'` (initial), `'sending'`, and `'sent'`:

import { useState } from 'react';

export default function FeedbackForm() {

const \[text, setText] = useState('');

const \[status, setStatus] = useState('typing');

async function handleSubmit(e) {

e.preventDefault();

setStatus('sending');

await sendMessage(text);

setStatus('sent');

const isSending = status === 'sending';

const isSent = status === 'sent';

if (isSent) {

return

## Thanks for feedback!

How was your stay at The Prancing Pony?

disabled={isSending}

value={text}

onChange={e => setText(e.target.value)}

\


disabled={isSending}

{isSending &&

Sending...

}

You can still declare some constants for readability:

But they’re not state variables, so you don’t need to worry about them getting out of sync with each other.

### Avoid redundant state

If you can calculate some information from the component’s props or its existing state variables during rendering, you **should not** put that information into that component’s state.

For example, take this form. It works, but can you find any redundant state in it?

import { useState } from 'react';

export default function Form() {

const \[firstName, setFirstName] = useState('');

const \[lastName, setLastName] = useState('');

const \[fullName, setFullName] = useState('');

function handleFirstNameChange(e) {

setFirstName(e.target.value);

setFullName(e.target.value + ' ' + lastName);

function handleLastNameChange(e) {

setLastName(e.target.value);

setFullName(firstName + ' ' + e.target.value);

### Let’s check you in

value={firstName}

value={lastName}

Your ticket will be issued to: **{fullName}**

import { useState } from 'react';

export default function Form() {

const \[firstName, setFirstName] = useState('');

const \[lastName, setLastName] = useState('');

const fullName = firstName + ' ' + lastName;

function handleFirstNameChange(e) {

setFirstName(e.target.value);

}

function handleLastNameChange(e) {

setLastName(e.target.value);

}

### Let’s check you in

value={firstName}

value={lastName}

Your ticket will be issued to: **{fullName}**

Here, `fullName` is _not_ a state variable. Instead, it’s calculated during render:

As a result, the change handlers don’t need to do anything special to update it. When you call `setFirstName` or `setLastName`, you trigger a re-render, and then the next `fullName` will be calculated from the fresh data.

### Avoid duplication in state

This menu list component lets you choose a single travel snack out of several:

import { useState } from 'react';

{ title: 'pretzels', id: 0 },

{ title: 'crispy seaweed', id: 1 },

{ title: 'granola bar', id: 2 },

export default function Menu() {

const \[items, setItems] = useState(initialItems);

const \[selectedItem, setSelectedItem] = useState(

items\[0]

### What's your travel snack?

{items.map(item => (

*   {item.title}

    {' '}

    \<button onClick={() => {

    setSelectedItem(item);

    \}}>Choose

    You picked {selectedItem.title}.

    Currently, it stores the selected item as an object in the `selectedItem` state variable. However, this is not great: **the contents of the `selectedItem` is the same object as one of the items inside the `items` list.** This means that the information about the item itself is duplicated in two places.

    Why is this a problem? Let’s make each item editable:

    import { useState } from 'react';

    { title: 'pretzels', id: 0 },

    { title: 'crispy seaweed', id: 1 },

    { title: 'granola bar', id: 2 },

    export default function Menu() {

    const \[items, setItems] = useState(initialItems);

    const \[selectedItem, setSelectedItem] = useState(

    items\[0]

    function handleItemChange(id, e) {

    setItems(items.map(item => {

    if (item.id === id) {

    title: e.target.value,

    } else {

    }));

    ### What's your travel snack?

    {items.map((item, index) => (
*   value={item.title}

    onChange={e => {

    Notice how if you first click “Choose” on an item and _then_ edit it, **the input updates but the label at the bottom does not reflect the edits.** This is because you have duplicated state, and you forgot to update `selectedItem`.

    Although you could update `selectedItem` too, an easier fix is to remove duplication. In this example, instead of a `selectedItem` object (which creates a duplication with objects inside `items`), you hold the `selectedId` in state, and _then_ get the `selectedItem` by searching the `items` array for an item with that ID:

    import { useState } from 'react';

    { title: 'pretzels', id: 0 },

    { title: 'crispy seaweed', id: 1 },

    { title: 'granola bar', id: 2 },

    export default function Menu() {

    const \[items, setItems] = useState(initialItems);

    const \[selectedId, setSelectedId] = useState(0);

    const selectedItem = items.find(item =>

    item.id === selectedId

    function handleItemChange(id, e) {

    setItems(items.map(item => {

    if (item.id === id) {

    title: e.target.value,

    }));

    ### What's your travel snack?

    {items.map((item, index) => (
*   (Alternatively, you may hold the selected index in state.)

    The state used to be duplicated like this:

    * `items = [{ id: 0, title: 'pretzels'}, ...]`
    * `selectedItem = {id: 0, title: 'pretzels'}`

    But after the change it’s like this:

    * `items = [{ id: 0, title: 'pretzels'}, ...]`
    * `selectedId = 0`

    The duplication is gone, and you only keep the essential state!

    Now if you edit the _selected_ item, the message below will update immediately. This is because `setItems` triggers a re-render, and `items.find(...)` would find the item with the updated title. You didn’t need to hold _the selected item_ in state, because only the _selected ID_ is essential. The rest could be calculated during render.

    ### Avoid deeply nested state

    Imagine a travel plan consisting of planets, continents, and countries. You might be tempted to structure its state using nested objects and arrays, like in this example:

    export const initialTravelPlan = {

    id: 0,

    title: '(Root)',

    childPlaces: \[{

    id: 1,

    title: 'Earth',

    childPlaces: \[{

    id: 2,

    title: 'Africa',

    childPlaces: \[{

    id: 3,

    title: 'Botswana',

    childPlaces: \[]

    id: 4,

    title: 'Egypt',

    childPlaces: \[]

    id: 5,

    title: 'Kenya',

    childPlaces: \[]

    id: 6,

    title: 'Madagascar',

    childPlaces: \[]

    id: 7,

    title: 'Morocco',

    childPlaces: \[]

    id: 8,

    title: 'Nigeria',

    childPlaces: \[]

    id: 9,

    title: 'South Africa',

    Now let’s say you want to add a button to delete a place you’ve already visited. How would you go about it? [Updating nested state](https://beta.reactjs.org/learn/updating-objects-and-arrays-in-state) involves making copies of objects all the way up from the part that changed. Deleting a deeply nested place would involve copying its entire parent place chain. Such code can be very verbose.

    **If the state is too nested to update easily, consider making it “flat”.** Here is one way you can restructure this data. Instead of a tree-like structure where each `place` has an array of _its child places_, you can have each place hold an array of _its child place IDs_. Then you can store a mapping from each place ID to the corresponding place.

    This data restructuring might remind you of seeing a database table:

    export const initialTravelPlan = {

    id: 0,

    title: '(Root)',

    childIds: \[1, 43, 47],

    id: 1,

    title: 'Earth',

    childIds: \[2, 10, 19, 27, 35]

    id: 2,

    title: 'Africa',

    childIds: \[3, 4, 5, 6 , 7, 8, 9]

    id: 3,

    title: 'Botswana',

    childIds: \[]

    id: 4,

    title: 'Egypt',

    childIds: \[]

    id: 5,

    title: 'Kenya',

    childIds: \[]

    id: 6,

    title: 'Madagascar',

    childIds: \[]

    **Now that the state is “flat” (also known as “normalized”), updating nested items becomes easier.**

    In order to remove a place now, you only need to update two levels of state:

    * The updated version of its _parent_ place should exclude the removed ID from its `childIds` array.
    * The updated version of the root “table” object should include the updated version of the parent place.

    Here is an example of how you could go about it:

    import { useState } from 'react';

    import { initialTravelPlan } from './places.js';

    export default function TravelPlan() {

    const \[plan, setPlan] = useState(initialTravelPlan);

    function handleComplete(parentId, childId) {

    const parent = planparentId;

    childIds: parent.childIds

    .filter(id => id !== childId)

    const root = plan\[0];

    const planetIds = root.childIds;

    ### Places to visit

    {planetIds.map(id => (

    key={id}

    id={id}

    onComplete={handleComplete}

    You can nest state as much as you like, but making it “flat” can solve numerous problems. It makes state easier to update, and it helps ensure you don’t have duplication in different parts of a nested object.

    Sometimes, you can also reduce state nesting by moving some of the nested state into the child components. This works well for ephemeral UI state that doesn’t need to be stored, like whether an item is hovered.

    ### Recap

    * If two state variables always update together, consider merging them into one.
    * Choose your state variables carefully to avoid creating “impossible” states.
    * Structure your state in a way that reduces the chances that you’ll make a mistake updating it.
    * Avoid redundant and duplicate state so that you don’t need to keep it in sync.
    * Don’t put props _into_ state unless you specifically want to prevent updates.
    * For UI patterns like selection, keep ID or index in state instead of the object itself.
    * If updating deeply nested state is complicated, try flattening it.

    This `Clock` component receives two props: `color` and `time`. When you select a different color in the select box, the `Clock` component receives a different `color` prop from its parent component. However, for some reason, the displayed color doesn’t update. Why? Fix the problem.

    export default function Clock(props) {

    const \[color, setColor] = useState(props.color);

    ##
