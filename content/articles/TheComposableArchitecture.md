# The Composable Architecture

These are some notes taken from the video series.

## Effects

Each effect has the power to choose the queue that it wants to run.
  
  The reason why we can this Architecture uniderectional data flow is because each efect that needs to mutate the state, needs to do it by sending a new action with the data to the store.
  All the mutations need to be done exclusively from inside the reducers.
    
 
  Another advantage is that the reducer is a pure function. It doesn`t execute effects, instead, returns an array of effects. This means it is moving the responsibility of executing and managing the effects to the colar of the function. 
    The place where the effects are being executed is the store.
    
    So an effect is an entity of generic types. However, the reducer returns always effects of an optinal type, being that type an action. This is to allow the result of the effect to be able to be feed again into the store via an action. 
    
    
    Effects per se run on same thread that creates the Effect. We may have effects that run in the main thread, asynchronously (or synchronously).
    However, if the need to run effects asynchronously (due to a network call or intensive computation), then we need to explicetely tell we want to receive the effect in the main.
      Some iOS APIs, such as URLSession, automaticaly calls back in a background thread. 
