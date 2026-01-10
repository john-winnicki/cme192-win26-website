---
layout: default
comments: false
keywords:

title: Schedule
description: CME 192 runs for eight lectures. This page lists weekly topics, suggested online prep, and materials.
buttons:
micro_nav: false
---

## Announcements
- Course materials and announcements will be posted to the course website and course GitHub.
- **Assignments:** two graded assignments (work individually or in teams).
  - **Assignment 1 due:** 02/15
  - **Assignment 2 due:** 03/15
- Questions or concerns: email the instructor (winnicki@stanford.edu).

## Schedule

- **Meeting time:** Thursdays, 4:30–5:20 PM (PT)
- **Location:** Hewlett Teaching Center 102
- **How to use this page:** The “Online modules to complete” are short MathWorks tutorials/onramps meant to get you up to speed for each lecture. Assignment due dates are highlighted in the schedule below.

<table id="schedule" class="table table-bordered no-more-tables" style="width: 100%; font-size: 0.8em;">
    <colgroup>
        <col>
        <col>
        <col style="width: 25%;">
        <col style="width: 25%;">
        <col style="width: 50%;">
    </colgroup>
    <thead class="active" style="background-color:#f9f9f9" align="left">
        <th>Event</th>
        <th>Date</th>
        <th>In-class lecture</th>
        <th>Online modules to complete</th>
        <th>Materials and Assignments</th>
    </thead>
    <tbody>
        <tr>
            <td>Lecture&nbsp;1</td>
            <td> 01/08 </td>
            <td>
                <strong>Topics:</strong> MATLAB Fundamentals (energy demand dataset)
                <ul>
                    <li>MATLAB setup + workflow (Desktop vs. MATLAB Online, Live Scripts)</li>
                    <li>Core data types: numeric arrays, cell arrays, structs</li>
                    <li>Working with tables + categorical arrays</li>
                    <li>Functions and function handles (@)</li>
                </ul>
            </td>
            <td>
                <strong>Required (MathWorks):</strong>
                <ul>
                    <li><a href="https://matlabacademy.mathworks.com/details/matlab-onramp/gettingstarted">MATLAB Onramp</a></li>
                    <li><a href="https://matlabacademy.mathworks.com/details/build-matlab-proficiency/lpmlbmp">Build MATLAB Proficiency (learning path)</a> (complete the first few items)</li>
                </ul>
                <strong>Recommended refreshers (MathWorks):</strong>
                <ul>
                    <li><a href="https://matlabacademy.mathworks.com/details/tables/otmltab">Tables</a></li>
                    <li><a href="https://www.mathworks.com/help/matlab/function-handles.html">Function Handles</a></li>
                </ul>
            </td>
            <td>
                <ul>
                    <li><a href="https://github.com/john-winnicki/cme192-win26/tree/main/lecture_01/livescript">Livescript</a></li>
                    <li><a href="https://drive.google.com/file/d/1AvNmw1YSRzSwfPJIsSmfffL5HszXGVRf/view?usp=drive_link">Slides</a></li>
                </ul>
            </td>
        </tr>

        <tr>
            <td>Lecture&nbsp;2</td>
            <td> 01/15 </td>
            <td>
                <strong>Topics:</strong> Advanced plotting &amp; visualizations (NHANES)
                <ul>
                    <li>Placing data and pre-setting axes (clean figure scaffolding)</li>
                    <li>Loading and merging the core NHANES tables</li>
                    <li>Plotting defaults: readable figures by construction</li>
                    <li>Histograms + scatter plots (incl. coloring by a third variable)</li>
                    <li>3D surfaces / response surfaces from scattered samples</li>
                    <li>Volume visualization (MRI example), exporting figures, and simple animations</li>
                </ul>
            </td>
            <td>
                <strong>Required (MathWorks):</strong>
                <ul>
                    <li><a href="https://matlabacademy.mathworks.com/details/explore-data-with-matlab-plots/otmledp">Explore Data with MATLAB Plots</a></li>
                </ul>
                <strong>Optional (MathWorks):</strong>
                <ul>
                    <li><a href="https://www.mathworks.com/discovery/data-visualization.html">Data Visualization with MATLAB</a></li>
                    <li><a href="https://matlabacademy.mathworks.com/details/clean-and-prepare-data-for-analysis/otmlpda">Clean and Prepare Data for Analysis</a></li>
                </ul>
            </td>
            <td>
                Coming soon!
            </td>
        </tr>

        <tr>
            <td>Lecture&nbsp;3</td>
            <td> 01/22 </td>
            <td>
                <strong>Topics:</strong> Applied mathematics in MATLAB (NLA + ODE/PDE + Symbolic)
                <ul>
                    <li>Numerical linear algebra demos: SVD/POD compression, sparse Poisson solves, eigenmodes (eigs), least squares + conditioning</li>
                    <li>ODE/PDE demos: diagnostics (vorticity/divergence), streamfunction Poisson solve, PDE Toolbox workflow, advection–diffusion time stepping</li>
                    <li>Symbolic Math: defining symbolic variables, manipulating expressions, and solving equations</li>
                </ul>
            </td>
            <td>
                <strong>Required (MathWorks):</strong>
                <ul>
                    <li><a href="https://matlabacademy.mathworks.com/details/solving-ordinary-differential-equations-with-matlab/odes">Solving Ordinary Differential Equations with MATLAB</a></li>
                    <li><a href="https://matlabacademy.mathworks.com/details/introduction-to-symbolic-math-with-matlab/symbolic">Introduction to Symbolic Math with MATLAB</a></li>
                </ul>
                <strong>Extra technical content (optional):</strong>
                <ul>
                    <li><a href="https://www.mathworks.com/moler/chapters.html">Numerical Computing with MATLAB (Cleve Moler) — web textbook</a></li>
                    <li><a href="https://www.mathworks.com/help/pde/getting-started-with-partial-differential-equation-toolbox.html">Get Started with Partial Differential Equation Toolbox</a></li>
                    <li><a href="https://www.mathworks.com/help/matlab/ref/ode45.html">ode45 documentation</a></li>
                    <li><a href="https://www.mathworks.com/help/matlab/ref/svd.html">SVD (svd)</a>, <a href="https://www.mathworks.com/help/matlab/ref/eigs.html">eigs</a>, <a href="https://www.mathworks.com/help/matlab/math/constructing-sparse-matrices.html">sparse matrices</a></li>
                </ul>
            </td>
            <td>
                Coming soon!
            </td>
        </tr>

        <tr>
            <td>Lecture&nbsp;4</td>
            <td> 01/29 </td>
            <td>
                <strong>Topics:</strong> File I/O, big data, and MATLAB &harr; Python/C++ workflows
                <ul>
                    <li>File manipulation + search path basics</li>
                    <li>Import/export: CSV/text, MAT-files; writing analysis results to tables (CSV/Excel)</li>
                    <li>Handling big data: memory hygiene, datastores + tall arrays, MapReduce, database workflows</li>
                    <li>Interoperability: calling Python from MATLAB, MATLAB from Python, and calling C/C++ (MEX / codegen)</li>
                </ul>
            </td>
            <td>
                <strong>Required (MathWorks):</strong>
                <ul>
                    <li><a href="https://matlabacademy.mathworks.com/details/import-data-from-multiple-files/otmlimf">Import Data from Multiple Files</a></li>
                    <li><a href="https://www.mathworks.com/help/matlab/import_export/tall-arrays.html">Tall Arrays for Out-of-Memory Data</a></li>
                </ul>
                <strong>Extra technical content (optional):</strong>
                <ul>
                    <li><a href="https://www.mathworks.com/help/parallel-computing/big-data-workflow-using-tall-arrays-and-datastores.html">Big Data Workflow Using Tall Arrays and Datastores</a></li>
                    <li><a href="https://www.mathworks.com/help/matlab/import_export/what-is-a-datastore.html">Getting Started with Datastore</a></li>
                    <li><a href="https://www.mathworks.com/help/matlab/import_export/getting-started-with-mapreduce.html">Getting Started with MapReduce</a></li>
                    <li><a href="https://www.mathworks.com/help/matlab/matlab_external/ways-to-call-python-from-matlab.html">Directly Call Python from MATLAB</a> and <a href="https://www.mathworks.com/help/matlab/matlab_external/get-started-with-matlab-engine-for-python.html">MATLAB Engine API for Python</a></li>
                    <li><a href="https://matlabacademy.mathworks.com/details/matlab-coder-onramp/ormc">MATLAB Coder Onramp</a> and <a href="https://www.mathworks.com/help/matlab/call-mex-files-1.html">Write C/C++ MEX Files</a></li>
                </ul>
            </td>
            <td>
                Coming soon!
            </td>
        </tr>

        <tr>
            <td>Lecture&nbsp;5</td>
            <td> 02/05 </td>
            <td>
                <strong>Topics:</strong> Statistics &amp; machine learning in MATLAB (classic ML + deep learning)
                <ul>
                    <li>Statistics and Machine Learning Toolbox: data preprocessing + feature engineering with tables</li>
                    <li>Unsupervised learning: PCA / dimension reduction + clustering (k-means, hierarchical clustering, GMM)</li>
                    <li>Example: grouping/clustering basketball players from tabular stats</li>
                    <li>Supervised ML: classification (heart disease demo) + regression basics</li>
                    <li>Model evaluation + improvement: train/test splits, confusion matrices, and hyperparameter tuning</li>
                    <li>Deep Learning Toolbox: pretrained CNNs, activations/feature extraction, and transfer learning (image classification demo)</li>
                    <li>Neural operators (FNO) demo: Python ↔ MATLAB workflow for PDE surrogate modeling</li>
                </ul>
            </td>
            <td>
                <strong>Required (MathWorks):</strong>
                <ul>
                    <li><a href="https://matlabacademy.mathworks.com/details/statistics-onramp/orst">Statistics Onramp</a></li>
                    <li><a href="https://matlabacademy.mathworks.com/details/machine-learning-onramp/machinelearning">Machine Learning Onramp</a></li>
                </ul>
                <strong>Optional (MathWorks):</strong>
                <ul>
                    <li><a href="https://matlabacademy.mathworks.com/details/deep-learning-onramp/deeplearning">Deep Learning Onramp</a> (recommended if you want to follow the deep learning demo live)</li>
                </ul>
                <strong>Extra technical content (optional):</strong>
                <ul>
                    <li><a href="https://www.mathworks.com/help/stats/clustering.html">Clustering (Statistics and Machine Learning Toolbox)</a> and <a href="https://www.mathworks.com/help/stats/principal-component-analysis-pca.html">PCA</a></li>
                    <li><a href="https://www.mathworks.com/help/stats/classification-learner-app.html">Classification Learner app</a></li>
                    <li><a href="https://www.mathworks.com/help/deeplearning/ug/transfer-learning.html">Transfer learning workflows in MATLAB</a></li>
                    <li><a href="https://arxiv.org/abs/2010.08895">Fourier Neural Operator (paper)</a></li>
                </ul>
            </td>
            <td>
                Coming soon!
            </td>
        </tr>

        <tr>
            <td>Lecture&nbsp;6</td>
            <td> 02/12 </td>
            <td>
                <strong>Topics:</strong> Optimization &amp; simulation/modeling (financial data demo)
                <ul>
                    <li>Optimization basics: Jacobians, gradients, Hessians</li>
                    <li>Root finding and nonlinear solves (bisection/Newton intuition; <code>fsolve</code>)</li>
                    <li>Unconstrained optimization (<code>fminunc</code>) and constrained optimization (<code>fmincon</code>)</li>
                    <li>Linear, quadratic, and integer programming (<code>linprog</code>, <code>quadprog</code>, <code>intlinprog</code>)</li>
                    <li>Nonlinear least squares (Gauss–Newton / Levenberg–Marquardt)</li>
                    <li>Applied example: calibrating a simple Black–Scholes model via least squares</li>
                </ul>
            </td>
            <td>
                <strong>Required (MathWorks):</strong>
                <ul>
                    <li><a href="https://matlabacademy.mathworks.com/details/optimization-onramp/optim">Optimization Onramp</a></li>
                </ul>
                <strong>Extra technical content (optional):</strong>
                <ul>
                    <li><a href="https://www.mathworks.com/help/optim/">Optimization Toolbox documentation</a></li>
                    <li><a href="https://web.stanford.edu/~boyd/cvxbook/">Convex Optimization (Boyd &amp; Vandenberghe) — free PDF</a></li>
                    <li><a href="https://www.mathworks.com/help/optim/ug/constrained-nonlinear-optimization-algorithms.html">Constrained nonlinear optimization algorithms (overview)</a></li>
                </ul>
            </td>
            <td>
                Coming soon!
            </td>
        </tr>

        <tr>
            <td id="Assignment_1" colspan="5" style="text-align:center; vertical-align:middle;background-color:#fffde7">
                <strong>Assignment 1</strong> (Due 02/15)
            </td>
        </tr>

        <tr>
            <td>Lecture&nbsp;7</td>
            <td> 02/26 </td>
            <td>
                <strong>Topics:</strong> Image processing in MATLAB (Astronomy edition)
                <ul>
                    <li>Dataset prep: SDSS “postage stamp” image cutouts (includes a Python download script)</li>
                    <li>Import, visualize, and compare images (<code>imshow</code>, montage, checkerboard)</li>
                    <li>Preprocessing: resize/crop borders, grayscale vs. color channels</li>
                    <li>Registration + difference imaging</li>
                    <li>Intensity profiles (“toy photometry”) and histograms</li>
                    <li>Contrast enhancement: <code>imadjust</code>, <code>histeq</code>, <code>adapthisteq</code></li>
                    <li>First-pass source detection: thresholding + morphology + background subtraction</li>
                    <li>(Optional) Semantic segmentation (SOURCE vs SKY) with a small deep net</li>
                </ul>
            </td>
            <td>
                <strong>Required (MathWorks):</strong>
                <ul>
                    <li><a href="https://matlabacademy.mathworks.com/details/image-processing-onramp/imageprocessing">Image Processing Onramp</a></li>
                </ul>
                <strong>Optional (MathWorks):</strong>
                <ul>
                    <li><a href="https://matlabacademy.mathworks.com/details/image-processing-with-matlab/mlip">Image Processing with MATLAB</a> (deeper dive)</li>
                    <li><a href="https://matlabacademy.mathworks.com/details/deep-learning-onramp/deeplearning">Deep Learning Onramp</a> (helpful for the optional segmentation section)</li>
                </ul>
                <strong>Extra technical content (optional):</strong>
                <ul>
                    <li><a href="https://www.mathworks.com/help/images/getting-started-with-image-processing-toolbox.html">Get Started with Image Processing Toolbox</a></li>
                    <li><a href="https://www.mathworks.com/help/images/morphological-filtering.html">Morphological filtering (imopen/imclose, etc.)</a></li>
                    <li><a href="https://www.sdss.org/">Sloan Digital Sky Survey (SDSS)</a> (dataset background)</li>
                </ul>
            </td>
            <td>
                Coming soon!
            </td>
        </tr>

        <tr>
            <td>Lecture&nbsp;8</td>
            <td> 03/13 </td>
            <td>
                <strong>Topics:</strong> Signal processing in MATLAB (earthquake + audio examples)
                <ul>
                    <li>Signal Processing Toolbox overview and a practical signal processing “pipeline”</li>
                    <li>Working with time-stamped signals via timetables; aligning and synchronizing signals</li>
                    <li>Spectral analysis: power spectra with <code>pspectrum</code> and frequency-domain interpretation</li>
                    <li>Preprocessing: resampling, smoothing, detrending</li>
                    <li>Filtering and basic feature extraction (peaks, envelopes, changepoints, similarity)</li>
                    <li>Quick tour of related toolboxes (Audio Toolbox, DSP System Toolbox)</li>
                </ul>
            </td>
            <td>
                <strong>Required (MathWorks):</strong>
                <ul>
                    <li><a href="https://matlabacademy.mathworks.com/details/signal-processing-onramp/signalprocessing">Signal Processing Onramp</a></li>
                </ul>
                <strong>Optional (MathWorks):</strong>
                <ul>
                    <li><a href="https://www.mathworks.com/help/signal/getting-started-with-signal-processing-toolbox.html">Get Started with Signal Processing Toolbox</a></li>
                    <li><a href="https://matlabacademy.mathworks.com/details/signal-classification-with-deep-learning/otmlscdl">Signal Classification with Deep Learning</a> (optional extension)</li>
                </ul>
                <strong>Extra technical content (optional):</strong>
                <ul>
                    <li><a href="https://www.mathworks.com/help/signal/ref/pspectrum.html">pspectrum documentation</a> and <a href="https://www.mathworks.com/help/signal/ref/spectrogram.html">spectrogram documentation</a></li>
                    <li><a href="https://www.mathworks.com/help/signal/ug/designing-filters-with-filter-designer.html">Design filters with Filter Designer</a></li>
                </ul>
            </td>
            <td>
                Coming soon!
            </td>
        </tr>

        <tr>
            <td id="Assignment_2" colspan="5" style="text-align:center; vertical-align:middle;background-color:#fffde7">
                <strong>Assignment 2</strong> (Due 03/15)
            </td>
        </tr>

    </tbody>
</table>
