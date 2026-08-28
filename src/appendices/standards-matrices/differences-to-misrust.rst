Analysis of Difference to MISRust
===================================

The tables provide an analysis of the difference between the mapping that the Safety Critical Consortium did
and the mapping done in `MISRust`_.

MISRust uses a mapping system that can be seen in Figure 2 of the paper.

.. list-table::
   :header-rows: 1
   :widths: auto

   * - Name
     - Meaning
   * - C1
     - C++ Std Library ``-`` Not applicable
   * - C2
     - C++ Specific ``-`` Not applicable
   * - C3
     - Not applicable
   * - C4
     - applies to unsafe rust
   * - C5
     - applies to unsafe rust ``-`` requires adaptation (Does this actually mean applies to unsafe with adaption? or is also safe code included??)
   * - C6
     - applies to safe rust

.. _misrust: https://arxiv.org/abs/2605.23490

Table 1 ``-`` Different mapping
-----------------------------

.. list-table::
   :header-rows: 1
   :widths: auto

   * - Guideline
     - mapping by SCRC
     - mapping by misrust
     - analysis
   * - Rule 0.0.1
     - applies to safe rust
     - C3
     - MISRust considers warnings here. I don't agree with this, as lints still have to be enabled, have no guarantee of completeness and C++ also has lints
   * - Rule 0.2.1
     - applies to safe rust
     - C3
     - consideration of warnings
   * - Rule 0.2.2
     - applies to safe rust
     - C3
     - consideration of warnings. additional rationale about virtual functions is wrong, because of traits
   * - Rule 0.2.3
     - applies to safe rust
     - C3
     - consideration of warnings
   * - Rule 4.1.1
     - applies to safe rust
     - C5
     - we consider unstable features similar to implementation specific features
   * - Rule 4.1.3
     - applies to safe rust
     - C4
     - we follow the MISRA C mapping here. Otherwise agree with the MISRust mapping
   * - Rule 4.6.1
     - does not apply to rust
     - C4
     - I think they misunderstood the MISRA rule. Rust does not have unsequenced operations.
   * - Rule 5.7.1
     - applies to safe rust
     - C3
     - we followed the MISRA C mapping. review needed.
   * - Rule 5.10.1
     - applies to safe rust
     - C3
     - review needed. should probably not apply.
   * - Rule 5.13.4
     - applies to safe rust
     - C3
     - review needed. while rust does not have implicit promotion, rust does support type inference, which for literals behaves similar.
   * - Rule 6.0.2
     - does not apply to rust
     - C6
     - From the rationale and example it seems like they wanted to say C3?? idk
   * - Rule 6.2.2
     - applies to unsafe rust
     - C3
     - we follow the misra C mapping. review especially regarding FFI needed
   * - Rule 6.2.4
     - does not apply to rust
     - C4
     - we put this rule into the more general case of rule 6.2.1
   * - Rule 6.4.2
     - does not apply to rust
     - C5
     - I don't understand how the example applies to this rule.
   * - Rule 6.7.1
     - applies to safe rust
     - C4
     - they interpret the MISRA rule to mostly be about the UB and not about hard to understand behaviour
   * - Rule 6.7.2
     - applies to safe rust
     - C4
     - same as Rule 6.7.1
   * - Rule 6.8.2
     - applies to unsafe rust
     - C3
     - Unsure if they consider the warning enough or if they say that since the unsafe is in the outer scope this does not apply
   * - Rule 6.8.3
     - applies to unsafe rust
     - C3
     - MISRust gives no rationale for not applying to unsafe code. I think they didn't consider pointers?
   * - Rule 7.0.3
     - does not apply to rust
     - C4
     - they don't consider the lack of implicit conversions enough and consider transmuting
   * - Rule 7.0.4
     - does not apply to rust
     - C4
     - I don't understand their rationale. For shifting the size of the rhs does not matter. Also shifting a u8 by a u128 is implemented, so i think that rationale is just wrong. The example also is wrong, the compile error is just a lint that is easily circumvented: https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=5ec5137598530cf55c1724ed344f20a8
   * - Rule 7.0.6
     - does not apply to rust
     - C6
     - Their example does not use assignement, but addition. Just because it also assigns does not mean that type conversion occurs
   * - Rule 7.11.1
     - does not apply to rust
     - C4
     - I don't understand how the given rationale applies to the rule.
   * - Rule 8.1.2
     - applies to unsafe rust
     - C3
     - it seems like they didn't consider capturing raw pointers?
   * - Rule 8.2.2
     - applies to safe rust
     - C3
     - needs review. We put emphasis on missing intent
   * - Rule 8.2.8
     - applies to safe rust
     - C4
     - needs review. We put emphasis on the data loss
   * - Rule 8.9.1
     - applies to safe rust
     - C4
     - the rationale is wrong. comparison between pointers is possible in safe code. Their example even does it in safe code.
   * - Rule 9.4.2
     - applies to safe rust
     - C3
     - MISRust does not map the subrules explicitly, so unsure if they would agree.
   * - Rule 9.5.1
     - applies to safe rust
     - C3
     - Their rationale does not consider while loops, that have a mutable loop variable.
   * - Rule 10.2.1
     - applies to safe rust
     - C3
     - needs review
   * - Rule 10.2.3
     - applies to safe rust
     - C3
     - Does not consider as casts or transmute
   * - Rule 11.3.2
     - applies to safe rust
     - C4
     - needs review. They don't consider multiple levels of reference indirection.
   * - Rule 13.3.4
     - applies to safe rust
     - C2
     - does not consider the broader issue of function/vtable pointer comparisons.
   * - Rule 15.0.1
     - applies to safe rust
     - C5
     - No rationale given why it would only map to unsafe rust
   * - Rule 15.1.4
     - applies to unsafe rust
     - C3
     - no rationale is given why it does not map to unsafe rust
   * - Rule 15.8.1
     - applies to unsafe rust
     - C3
     - needs review. Since rust has no better facility of writing move constructors, if they are required the same issues occur. Maybe this MISRA rule is mostly concerned with the implicit nature of the move operation.
   * - Rule 18.1.1
     - applies to unsafe rust
     - C2
     - rust has the same exception model as C++. Classification in recoverable and not recoverable is just convention and a project could define a different convention.
   * - Rule 18.4.1
     - applies to safe rust
     - C2
     - I don't understand their rationale at all. Does not consider extern "C"
   * - Rule 18.5.1
     - applies to safe rust
     - C5
     - Does not consider panics. No rationale given why this would only apply to unsafe and not also safe rust.
   * - Rule 19.0.1
     - does not apply to rust
     - C5
     - needs review. good idea with a misspelling. no rationale for why only unsafe
   * - Rule 19.0.2
     - applies to safe rust
     - C2
     - The example they give does not actually show type safety in macros. see https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=49e888f66967df097b8f742fffca8744. example t shows lack of type safety and example t2 shows issue of multiple evaluation of argument which they ignored.
   * - Rule 19.1.3
     - does not apply to rust
     - C4
     - The rule is concerned with fallback to 0. Rust stopping the compilation is safe behaviour.
   * - Rule 19.2.1
     - applies to safe rust
     - C3
     - needs review. they did not consider the include macro in their rationale
   * - Rule 19.2.2
     - does not apply to rust
     - C5
     - no ub in rust
   * - Rule 19.2.3
     - does not apply to rust
     - C4
     - no implementation defined behaviour in rust. no rationale given for why this would not also map to safe rust
   * - Rule 19.3.4
     - applies to safe rust
     - C4
     - no rationale given why this only applies to unsafe rust. also does not consider the difference between multiple macro parameter types.
   * - Rule 21.2.3
     - applies to safe rust
     - C1
     - no consideration that the equivalent API in rust may have the same/similar issues.
   * - Rule 21.6.2
     - applies to safe rust
     - C4
     - needs review. does not consider creation of ManuallyDrop or usage of forget/leak.
   * - Rule 21.6.3
     - applies to safe rust
     - C4
     - needs review.
   * - Rule 21.10.1
     - applies to unsafe rust
     - C1
     - rust supports C-variadic args the same way with the same issue as C++
   * - Rule 21.10.2
     - applies to unsafe rust
     - C1
     - rust has no equivalent feature therefore we though about calling the C++ functions using FFI. They didn't consider this
   * - Rule 21.10.3
     - applies to unsafe rust
     - C1
     - rust has no equivalent feature therefore we though about calling the C++ functions using FFI. They didn't consider this
   * - Rule 22.3.1
     - applies to safe rust
     - C5
     - why does this need to be adapted? only the macro names differ
   * - Rule 22.4.1
     - applies to unsafe rust
     - C2
     - feature does exist in rust. errno can be read using a std function.
   * - Rule 23.11.1
     - applies to unsafe rust
     - C2
     - rationale sounds like they wanted to map this to unsafe rust?
   * - Rule 24.5.2
     - applies to unsafe rust
     - C1
     - no rationale given why the UB in case of overlap does not apply to rust.
   * - Rule 25.5.1
     - applies to unsafe rust
     - C1
     - since rust does not provide locale support we thought about using FFI to use this feature. They didn't consider this
   * - Rule 25.5.2
     - applies to unsafe rust
     - C1
     - since rust does not provide locale support we thought about using FFI to use this feature. They didn't consider this
   * - Rule 25.5.3
     - applies to unsafe rust
     - C1
     - since rust does not provide support for some of these feautures we considered FFI. they didn't consider this
   * - Rule 28.3.1
     - applies to safe rust
     - C2
     - no rationale given why the issues regarding state in predicates don't apply.
   * - Rule 28.6.3
     - applies to unsafe rust
     - C2
     - no consideration given to unsafe rust
   * - Rule 28.6.4
     - applies to unsafe rust
     - C2
     - no actual rationale for the mapping given

Table 2 ``-`` Same mapping
------------------------
* Rule 0.0.2
* Rule 0.1.1
* Rule 0.1.2
* Dir 0.3.1
* Dir 0.3.2
* Rule 4.1.2
* Rule 5.0.1
* Dir 5.7.2
* Rule 5.7.3
* Rule 5.13.1
* Rule 5.13.2
* Rule 5.13.3
* Rule 5.13.5
* Rule 5.13.6
* Rule 5.13.7
* Rule 6.0.1
* Rule 6.0.3
* Rule 6.0.4
* Rule 6.2.1
* Rule 6.2.3
* Rule 6.4.1
* Rule 6.4.3
* Rule 6.5.1
* Rule 6.5.2
* Rule 6.8.1
* Rule 6.8.4
* Rule 6.9.1
* Rule 6.9.2
* Rule 7.0.1
* Rule 7.0.2
* Rule 7.0.5
* Rule 7.11.2
* Rule 7.11.3
* Rule 8.1.1
* Rule 8.2.1
* Rule 8.2.3
* Rule 8.2.4
* Rule 8.2.5
* Rule 8.2.6
* Rule 8.2.7
* Rule 8.2.9
* Rule 8.2.10
* Rule 8.2.11
* Rule 8.3.1
* Rule 8.3.2
* Rule 8.7.1
* Rule 8.7.2
* Rule 8.14.1
* Rule 8.18.1
* Rule 8.18.2
* Rule 8.19.1
* Rule 8.20.1
* Rule 9.2.1
* Rule 9.3.1
* Rule 9.4.1
* Rule 9.5.2
* Rule 9.6.1
* Rule 9.6.2
* Rule 9.6.3
* Rule 9.6.4
* Rule 9.6.5
* Rule 10.0.1
* Rule 10.1.2
* Rule 10.2.2
* Rule 10.3.1
* Rule 10.4.1
* Rule 11.3.1
* Rule 11.6.1
* Rule 11.6.2
* Rule 11.6.3
* Rule 12.2.1
* Rule 12.2.2
* Rule 12.3.1
* Rule 13.1.1
* Rule 13.1.2
* Rule 13.3.1
* Rule 13.3.2
* Rule 13.3.3
* Rule 14.1.1
* Rule 15.0.2
* Rule 15.1.1
* Rule 15.1.2
* Rule 15.1.3
* Rule 15.1.5
* Rule 16.5.1
* Rule 16.5.2
* Rule 16.6.1
* Rule 17.8.1
* Rule 18.1.2
* Rule 18.3.1
* Rule 18.3.2
* Rule 18.3.3
* Rule 18.5.2
* Rule 19.0.3
* Rule 19.0.4
* Rule 19.1.1
* Rule 19.1.2
* Rule 19.3.1
* Rule 19.3.2
* Rule 19.3.3
* Rule 19.3.5
* Rule 19.6.1
* Rule 21.2.1
* Rule 21.2.2
* Rule 21.2.4
* Rule 21.6.1
* Rule 21.6.4 although their rationale is wrong. rust allows overwriting the global allocator
* Rule 21.6.5 although we should probably not map this to rust
* Rule 24.5.1
* Rule 26.3.1
* Rule 28.6.1
* Rule 28.6.2
* Rule 30.0.1
* Rule 30.0.2
