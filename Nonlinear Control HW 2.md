## HW2

Topic: Input-Output Linearization
Keywords: relative degree, zero dynamics, Lie derivative, diffeomorphism, coordinate transformation

- Test observability.
    - LTI: differentiate wrt time; if the observability pair is not full rank, the input doe not appear in the highest derivative of the output and the higher order terms are unobservable
    - NLTI: use Lie derivative; if the relative degree is not the dimension of the state space, the input dose not appear in the highest Lie derivative of the output and some of the terms are unobservable
- The unobservable modes are referred to as the internal dynamics
- The zero dynamics are the dynamics of the reduced system defined as the internal dynamics with observable modes set to zero.
- Change coordinates
    - Convert the system to normal form by setting the normal state vector as the Lie derivatives of the observable modes, followed by functions of the original state.
    - The Lie derivative of these other functions with the input vector function must be zero.
    - These other functions must be functions of the controllable modes to complete the transformation.
