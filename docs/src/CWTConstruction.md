# CWT Construction
```@docs
CWT
ContinuousWavelets.wavelet(::WC) where {WC<:ContinuousWavelets.ContWaveClass}
```

The `ContWaveClass` type defines the kind of mother and father wavelet function. 
The `CWT` type, in contrast, defines everything else that goes into performing a continuous wavelet transform besides that choice.
The function `wavelet()` has been overloaded to work with ContWaveClass in much the same way it works for the `owt`s of Wavelets.jl.
In more detail, the parameters, along with their defaults, are:
+ `wave::ContWaveClass`: is a type of continuous wavelet, see the [Available Wavelet Families](@ref).
+ `scalingFactor`, `s`, or `Q::Real=8.0`: the number of wavelets between the octaves ``2^J`` and ``2^{J+1}`` (defaults to 8, which is most appropriate for music and other audio). Valid range is $(0,\infty)$.
+ `β::Real=4.0`: As using exactly `Q` wavelets per octave leads to excessively many low-frequency wavelets, `β` varies the number of wavelets per octave, with larger values of `β` corresponding to fewer low frequency wavelets(see [Wavelet Frequency Spacing](@ref) for details).
  Valid range is $(1,\infty)$, though around `β=6` the spacing is approximately linear *in frequency*, rather than log-frequency, and begins to become concave after that.
+ `boundary::WaveletBoundary=SymBoundary()`: The default boundary condition is `SymBoundary()`, implemented by appending a flipped version of the vector at the end to eliminate edge discontinuities. See [Boundary Conditions](@ref) for the other possibilities. 
+ `averagingType::Average=Father()`: determines whether or not to include the
  averaging function, and if so, what kind of averaging. The options are
  - `Father`: use the averaging function that corresponds to the mother Wavelet.
  - `Dirac`: use the sinc function with the appropriate width.
  - `NoAve`: don't average. this has one fewer filters than the other `averagingTypes`
+ `averagingLength::Int=4`:  the number of wavelet octaves that are covered by the averaging, 
+ `frameBound::Real=1`: gives the total norm of the whole collection, corresponding to the upper frame bound; if you don't want to impose a particular bound, set `frameBound<0`.
+ `normalization` or `p::Real=Inf`: the p-norm preserved as the scale changes, so if we're scaling by $s$, `normalization` has value `p`, and the mother wavelet is $\psi$, then the resulting wavelet is $s^{1/p}\psi(^{t}/_{s})$.
  The default scaling, `Inf` gives all the same maximum value in the frequency domain.
  Valid range is $(0,\infty]$, though $p<1$ isn't actually preserving a norm.
