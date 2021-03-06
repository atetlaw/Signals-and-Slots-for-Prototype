Signals and slots allows you to register a function to be called whenever another function is called. It goes beyond DOM event listeners and allows you to turn any function into an event. The function you are listening for is called the 'signal' and any functions whho are listeners are called 'slots'. So when the signal function is applied, all the slot functions are also applied, typically with the same arguments as the signal function.

The main benefit of doing things this way is that it becomes an anonymous transport mechanism that allows you to pass information between objects without the objects having to know about each other. A lot of Prototype objects take functions as arguments for callback purposes. Signals and slots negates the need for callbacks. I demonstrated this in the entry Signals and Slots is King. And it potentially makes for cleaner simpler code.

##API Usage 

This is the first release and I've implemented a simple set of functions. 

    Signal.connect(object1,'function1Name',object2,'function2Name');
This is the main function. You pass an object reference and the name of a function of that object as a string; this is the signal. The second pair represents a slot. The above function means that whenever object1['function1Name'](arguments) is called, object2['function2Name'](arguments) will also be called with the same arguments.

If either object1 or object2 is null then it will default to the global window object, allowing you to connect functions in the global namespace.

You can keep on connecting to object1['function1Name'], in other words adding more slots, and they will all be called one after the other, in the order they were connecteded whenever object1['function1Name'](arguments) is called.

Using Signal.connect() there are no limits to the number of tims you can connect the same functions. So there is an alternative:

    Signal.connectOnce(object1,'function1Name',object2,'function2Name');
This will ensure that object2['function2Name'] is only connected as a slot once.

There are two disconnect functions that allow you to break the connection between signal and slot.

    Signal.disconnect(object1,'function1Name',object2,'function2Name');
Call this with the same arguments as when you connected and the connection will be destroyed. If the same function exists as multiple slot this will only destroy connections in the order they were made; that is, first in, first out.

    Signal.disconnectAll(object1,'function1Name',object2,'function2Name');
Call this function to disconnect all slots from the signal matching object2['function2Name']. This is useful when you can't guarrantee how many times a slot has been connected to a signal but you want to be sure to destroy all of them.

There are some options available as well. If you pass {before:true} the slot function will be called before the signal function:

    Signal.connect(object1,'function1Name',object2,'function2Name', {before:true});
You can also pass an optional mutator function. This must be a function that returns an array matching the javascript arguments array. The returned array will be passed to the slot function instead of the default arguments array that was passed to the signal function. Use this to modify the arguments from signal to slot. For example:

    Signal.connect(object1,'function1Name',object2,'function2Name', {mutate:function() { return [arguments[0]*2]}});
In the above example, if the signal function was called with an argument of '2' then the slot will be called with an argument of '4'.
