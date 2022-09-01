# Aim of this tutorial

This short tutorial gives an overview of history matching with emulation and shows how to implement this technique in a one-dimensional example using the HMER package.

Computer models, otherwise known as simulators, have been widely used in almost all fields in science and technology. This includes the use of infectious disease models in epidemiology and public health. A computer model is a way of representing the fundamental dynamics of a system. Due to the complexity of the interactions within a system, computer models frequently contain large numbers of parameters.

Before using a model for projection or planning, it is fundamental to explore plausible values for its parameters, calibrating the model to the observed data. This poses a significant problem, considering that it may take several minutes or even hours for the evaluation of a single scenario of a complex model. This difficulty is compounded for stochastic models, where hundreds or thousands of realisations may be required for each scenario. As a consequence, a comprehensive analysis of the entire input space, requiring vast numbers of model evaluations, is often unfeasible. Emulation, combined with history matching, allows us to overcome this issue.

## History Matching
History matching concerns the problem of identifying those parameter sets that may give rise to acceptable matches between the model outputs and the observed data. This part of the input space is referred to as non-implausible, while its complement is referred to as implausible. History matching proceeds as a series of iterations, called waves, where implausible areas of parameter space are identified and discarded. Each wave focuses the search for implausible space in the space that was characterized as non-implausible in all previous waves: thus the non-implausible space shrinks with each wave. To decide whether a parameter set  x is implausible we introduce the implausibility measure, which evaluates the difference between the model results and the observed data, weighted by how uncertain we are at x. If such measure is too high, the parameter set is discarded in the next wave of the process.
Note that history matching as just described still relies on the evaluation of the model at a large number of parameter sets, which is often unfeasible. Here is where emulators play a crucial role.

## Emulators
A long established method for handling computationally expensive models is to first construct an emulator: a fast statistical approximation of the model that can be used as a surrogate. In other words, we can think of an emulator as a way of representing our beliefs about the behaviour of a complex model. Note that one can either construct an emulator for each of the model output separately, or combine outputs together, through more advanced techniques. From here on we assume that each model output will have its own emulator.

The model is run at a manageable number of parameter sets to provide training data for the emulator. The emulator is then built and can be used to obtain an expected value of the model output at any parameter set  x, along with a corresponding uncertainty estimate reflecting our beliefs about the uncertainty in the approximation.

Emulators have two useful properties. First, they are computationally efficient - typically several orders of magnitude faster than the computer models they approximate. Second, they allow for the uncertainty in their approximations to be taken into account. These two properties mean that emulators can be used to make inferences as a surrogate for the model itself. In particular, when going through the history matching process, it is possible to evaluate the implausibility measure at any given parameter set by comparing the observed data to the emulator output, rather than the model output. This greatly speeds up the process and allows for a comprehensive exploration of the input space.


