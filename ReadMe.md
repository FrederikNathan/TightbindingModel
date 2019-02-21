# TightBindingModel
Python module for constructing and analyzing tight-binding models in solid-state physics (in progress)

The main module is called TightBindingModel (TBM). This contains the object representing tight-binding models (TBM.Hamiltonian), as well as general methods for these. It also contains definition objects that live in the Brillouin zone more generally (such as band structure, and general n x m-dimensional fields). 

The Module LatticeModel translates a tight binding Hamiltonian to a lattice Hamiltonian matrix, with given dimensionality and boundary conditions. Also contains useful objects and methods when working with lattice models, such as disorder.

The module BasicFunctions contains basic, useful functions such as logging etc. 


# TightBindingModel

To use:
Any tight-binding model is defined with a dimensionality, and orbital dimension. When using TightBindingModel (TBM), one should define the following, before calling any object or function:

TBM.Dimension = D
TBM.OrbitalDimension = X

where D is the physical dimension of the model, and X denotes the number of orbitals per unit cell. 

## BZO (Brillouin Zone Object): 
 
Syntax: BZO(*args,shape=None,dtype=complex) 
 
Fundamental object used to represent fields defined on a Brillouin zone. In particular, Hamiltonians are a subclass of a BZO. The BZO object stores the information of the field in a compact way, which enables fast execution of common methods.
 
Since a field on the BZ is a periodic function of crystal momentum, it can be decomposed into discrete harmonics (a Fourier series): F(k) = \sum_{abc} F_{abc} e^{-i (aq_1 +bq_2 + cq_3) \cdot k}.For example Hamiltonians have particularly simple harmonics, since the (i,j,k)th Harmonic corresponds to hopping between unit cells separated by vector  (ia_1,ja_2 ,ka_3), where  a_i denotes the ith basis vector of the lattice (normally only nearest-neighbour hopping is allowed, and hence only the 9 components  (\pm 1, \pm 1 , \pm 1), (0,0,0) are nonzero.  

In essence, the BZO represents a BZ field by storing a list of components {F_1 , \ldots F_n} (ObjList), along with their corresponding indices {(a_1,b_1,c_1),\ldots (a_n,b_n,c_n)} (IndList). 
 
The Harmonics \{F_{ijk}\} of a BZO can be accessed and set using the __set__ and __get__ method: 
 
BZO[i,j,k] =F_{ijk}. 
 
The value of F at a crystal momentum k (or a collection of crystal momenta) can be found using the __call__method (see __call__ method for how multiple k-points should be formatted): 
 
BZO(k) = F(k)
 
Currently, TBM only works for square lattices: by default q_i  is the ith unit vector. In a future version, the BZO can be updated to allow for general vectors).


### Defining a BZO:
A BZO can be defined in 3 different ways

#### Method 1: 
The BZO can be generated from an IndList and ObjList generated previously:

A = IndList

B = ObjList

X = BZO(A,B)

#### Method 2: 
The BZO is generated from an (m_1 x ... m_n) array of BZO's. Example:

A=<BZO_1>

B=<BZO_2>

C=array([A,B])

X=BZO(C)

#### Method 3: 
The BZO is defined as an empty object, and harmonics can be set afterwards. Here the shape should be specified (in the other two cases it should remain “None”). 

X=BZO(shape=(2,2),dtype=float)

X[1,2,3]=array([[1,2],[3,4]])

### Basic Methods

#### Set and Get 
The (i,j,k)th of a BZO object A can be accessed and set by calling A[i,j,k] (also generalizes to more dimensions). If BZO A is initially empty, 

#### Call
The value of a BZO can be evaluated at crystal momentum k, or at an array of crystal momenta Karray by calling the BZO: F(k) = \sum_{abc} F_{abc} e^{-i (aq_1 +bq_2 + cq_3) \cdot k}.

#### Arithmetics
Multiplication, addition and subtraction are defined between BZOs of the same shape. Here the operation returns the corresponding BZO. I.e., if A and B are the BZO representing the BZ field F(k) and G(k), A*B represents F(k)*G(k). The product of two BZOs is computed efficiently from IndList and ObjList. 

Moreover, scalar multiplication and division are defined. In Hamiltonian and Scalar subclasses, a scalar Scalar addition are also defined. 

### Advanced methods:

#### NNZ()
Returns number of nonzero harmonics (i.e. len(IndList)). 

#### ObjList()
Returns ObjList (ie. sorted list with Harmonics)

#### IndList()
Returns IndList (i.e. sorted list with planewaves corresponding to harmonics)

<More to come>
