---
title: "Knowledge Graph"
excerpt: ""
collection: projects4fun
header:
    teaser: 
skills:
    - Lyapunov stability
---
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lyapunov Stability Knowledge Graph</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- D3.js CDN -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/d3/7.8.5/d3.min.js"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            margin: 0;
            overflow: hidden; /* Prevent body scroll, D3 will handle zoom/pan */
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }
        .graph-container {
            flex-grow: 1;
            position: relative;
            background-color: #f8fafc; /* Tailwind: bg-slate-50 */
            border-radius: 0.75rem; /* Tailwind: rounded-xl */
            overflow: hidden; /* Ensure SVG doesn't bleed out */
        }
        svg {
            display: block;
            /* Ensure SVG takes full dimensions of its flex parent */
            width: 100%;
            height: 100%;
        }
        .node circle {
            stroke: #fff;
            stroke-width: 1.5px;
        }
        .node text {
            pointer-events: none;
            font-size: 10px; /* Smaller font for better fit */
            text-anchor: middle;
            fill: #334155; /* Tailwind: text-slate-700 */
            text-shadow: 0 0 2px white, 0 0 2px white, 0 0 2px white; /* White shadow for readability */
        }
        .link {
            stroke: #94a3b8; /* Tailwind: stroke-slate-400 */
            stroke-opacity: 0.6;
            stroke-width: 1.5px;
        }
        .tooltip {
            position: absolute;
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 8px 12px;
            border-radius: 6px;
            pointer-events: none;
            opacity: 0;
            transition: opacity 0.2s;
            font-size: 0.875rem;
            max-width: 250px;
            text-align: center;
            line-height: 1.4;
            box-shadow: 0 2px 10px rgba(0,0,0,0.2);
        }
    </style>
</head>
<body class="bg-gradient-to-br from-blue-50 to-indigo-100 p-4">
    <div class="max-w-7xl mx-auto w-full flex flex-col h-full bg-white rounded-xl shadow-lg p-6">
        <h1 class="text-3xl font-extrabold text-center text-blue-800 mb-6">
            Lyapunov Stability Knowledge Graph
        </h1>
        <p class="text-center text-gray-600 mb-8">
            Explore the interconnected concepts and methods related to Lyapunov stability theory.
            Drag nodes to rearrange, and use your mouse wheel to zoom. Hover over nodes for details.
        </p>

        <div id="graph" class="graph-container flex-grow">
            <!-- D3.js graph will be rendered here -->
        </div>

        <div id="node-details" class="mt-6 p-4 bg-blue-50 rounded-lg text-blue-800 text-sm">
            <h3 class="font-semibold mb-2">Node Information:</h3>
            <p id="selected-node-info">Hover over a node to see its details.</p>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Define graph data: nodes (concepts) and links (relationships)
            const graphData = {
                nodes: [
                    // Core Concepts
                    { id: 'Lyapunov_Function', group: 'Core_Concept', description: 'A scalar function used to analyze stability without solving differential equations.' },
                    { id: 'Stability', group: 'Core_Concept', description: 'A property of a dynamical system where trajectories remain close to an equilibrium point when perturbed.' },
                    { id: 'Asymptotic_Stability', group: 'Core_Concept', description: 'A stronger form of stability where trajectories not only remain close but also converge to the equilibrium point over time.' },
                    { id: 'Global_Asymptotic_Stability', group: 'Core_Concept', description: 'Asymptotic stability that applies to the entire state space.' },
                    { id: 'Equilibrium_Point', group: 'Core_Concept', description: 'A state where the system remains indefinitely if started there (f(x)=0).' },
                    { id: 'Dynamical_System', group: 'Core_Concept', description: 'A system whose state evolves over time according to a fixed rule.' },
                    { id: 'Linear_System', group: 'System_Type', description: 'A dynamical system whose equations are linear.' },
                    { id: 'Nonlinear_System', group: 'System_Type', description: 'A dynamical system whose equations are nonlinear.' },
                    { id: 'Time_Derivative_of_V', group: 'Concept_Property', description: 'The rate of change of the Lyapunov function along system trajectories (dV/dt).' },
                    { id: 'Positive_Definite', group: 'Math_Property', description: 'A function V(x) where V(0)=0 and V(x)>0 for x!=0.' },
                    { id: 'Negative_Definite', group: 'Math_Property', description: 'A function V(x) where V(0)=0 and V(x)<0 for x!=0.' },
                    { id: 'Negative_Semidefinite', group: 'Math_Property', description: 'A function V(x) where V(0)=0 and V(x)<=0 for x!=0.' },
                    { id: 'Radially_Unbounded', group: 'Math_Property', description: 'A function V(x) where V(x) goes to infinity as the norm of x goes to infinity.' },
                    { id: 'Chain_Rule', group: 'Math_Tool', description: 'A calculus rule used to find the time derivative of a composite function.' },

                    // Methods & Criteria
                    { id: 'Lyapunovs_Direct_Method', group: 'Method', description: 'Proves stability by directly constructing a Lyapunov function.' },
                    { id: 'Lyapunovs_Indirect_Method', group: 'Method', description: 'Analyzes stability by linearizing the system around an equilibrium point.' },
                    { id: 'Eigenvalue_Analysis', group: 'Method', description: 'Determines stability of linear systems based on the eigenvalues of the system matrix.' },
                    { id: 'Routh_Hurwitz_Criterion', group: 'Method', description: 'An algebraic method to check stability of linear systems from characteristic polynomial coefficients.' },
                    { id: 'Nyquist_Stability_Criterion', group: 'Method', description: 'A graphical frequency-domain method for feedback system stability analysis.' },
                    { id: 'Bode_Stability_Criterion', group: 'Method', description: 'Uses Bode plots to determine stability margins in the frequency domain.' },
                    { id: 'Root_Locus_Method', group: 'Method', description: 'A graphical method showing how pole locations change with gain, indicating stability.' },
                    { id: 'Phase_Portrait_Analysis', group: 'Method', description: 'Visualizes system trajectories in state space for qualitative stability analysis (1D/2D).' },
                    { id: 'Center_Manifold_Theory', group: 'Method', description: 'Analyzes stability in critical cases where linearization fails.' },
                    { id: 'Input_to_State_Stability', group: 'Method', description: 'A stability notion that considers the effect of external inputs.' },
                    { id: 'Backstepping', group: 'Method', description: 'A recursive design method for stabilizing controllers and Lyapunov functions for nonlinear systems.' },
                    { id: 'Control_Lyapunov_Function', group: 'Method', description: 'A type of Lyapunov function used to directly synthesize stabilizing control laws.' },
                    { id: 'Sum_of_Squares_Methods', group: 'Method', description: 'Computational methods for finding polynomial Lyapunov functions.' },
                    { id: 'Quadratic_Form', group: 'Math_Concept', description: 'A homogeneous polynomial of degree two, often used as a Lyapunov function candidate.' },
                    { id: 'Lyapunov_Equation', group: 'Math_Concept', description: 'A matrix equation (A^T P + PA = -Q) used to find quadratic Lyapunov functions for linear systems.' },
                    { id: 'Linearization', group: 'Process', description: 'Approximating a nonlinear system by a linear one around an equilibrium.' },
                    { id: 'Control_Laws', group: 'Output', description: 'Rules that determine how a system’s inputs should be adjusted to achieve desired behavior.' }
                ],
                links: [
                    // Core Concept Relationships
                    { source: 'Dynamical_System', target: 'Stability', type: 'HAS_PROPERTY' },
                    { source: 'Stability', target: 'Asymptotic_Stability', type: 'HAS_SUBTYPE' },
                    { source: 'Asymptotic_Stability', target: 'Global_Asymptotic_Stability', type: 'HAS_SUBTYPE' },
                    { source: 'Dynamical_System', target: 'Equilibrium_Point', type: 'HAS_CRITICAL_POINT' },
                    { source: 'Lyapunov_Function', target: 'Stability', type: 'IS_USED_FOR' },
                    { source: 'Lyapunov_Function', target: 'Positive_Definite', type: 'IS_A_PROPERTY' },
                    { source: 'Time_Derivative_of_V', target: 'Negative_Definite', type: 'IS_A_PROPERTY' },
                    { source: 'Time_Derivative_of_V', target: 'Negative_Semidefinite', type: 'IS_A_PROPERTY' },
                    { source: 'Lyapunov_Function', target: 'Radially_Unbounded', type: 'REQUIRES_FOR_GLOBAL' },
                    { source: 'Time_Derivative_of_V', target: 'Chain_Rule', type: 'IS_CALCULATED_USING' },
                    { source: 'Time_Derivative_of_V', target: 'Lyapunov_Function', type: 'IS_CALCULATED_FROM' }, // Adjusted for clarity
                    { source: 'Asymptotic_Stability', target: 'Negative_Definite', type: 'IMPLIES_V_DOT_IS' }, // Adjusted for clarity
                    { source: 'Stability', target: 'Negative_Semidefinite', type: 'IMPLIES_V_DOT_IS' }, // Adjusted for clarity

                    // Method & Criteria Relationships
                    { source: 'Lyapunovs_Direct_Method', target: 'Lyapunov_Function', type: 'USES' },
                    { source: 'Lyapunovs_Direct_Method', target: 'Nonlinear_System', type: 'APPLIES_TO' },
                    { source: 'Lyapunovs_Direct_Method', target: 'Linear_System', type: 'APPLIES_TO' },

                    { source: 'Lyapunovs_Indirect_Method', target: 'Linearization', type: 'INVOLVES' },
                    { source: 'Lyapunovs_Indirect_Method', target: 'Nonlinear_System', type: 'APPLIES_TO' },
                    { source: 'Lyapunovs_Indirect_Method', target: 'Eigenvalue_Analysis', type: 'USES' },

                    { source: 'Eigenvalue_Analysis', target: 'Linear_System', type: 'APPLIES_TO' },
                    { source: 'Routh_Hurwitz_Criterion', target: 'Linear_System', type: 'APPLIES_TO' },
                    { source: 'Nyquist_Stability_Criterion', target: 'Linear_System', type: 'APPLIES_TO' },
                    { source: 'Bode_Stability_Criterion', target: 'Linear_System', type: 'APPLIES_TO' },
                    { source: 'Root_Locus_Method', target: 'Linear_System', type: 'APPLIES_TO' },

                    { source: 'Phase_Portrait_Analysis', target: 'Nonlinear_System', type: 'APPLIES_TO' },
                    { source: 'Center_Manifold_Theory', target: 'Linearization', type: 'HANDLES_CRITICAL_CASES_OF' },
                    { source: 'Center_Manifold_Theory', target: 'Nonlinear_System', type: 'APPLIES_TO' },
                    { source: 'Input_to_State_Stability', target: 'Nonlinear_System', type: 'APPLIES_TO' },
                    { source: 'Input_to_State_Stability', target: 'External_Inputs', type: 'CONSIDERS' }, // Added External_Inputs node
                    { source: 'External_Inputs', target: 'Dynamical_System', type: 'AFFECTS' },

                    { source: 'Lyapunov_Function', target: 'Quadratic_Form', type: 'CAN_BE_A' },
                    { source: 'Quadratic_Form', target: 'Lyapunov_Equation', type: 'LEADS_TO' },

                    { source: 'Backstepping', target: 'Control_Lyapunov_Function', type: 'IS_A_DESIGN_METHOD_FOR' },
                    { source: 'Backstepping', target: 'Nonlinear_System', type: 'APPLIES_TO' },
                    { source: 'Control_Lyapunov_Function', target: 'Control_Laws', type: 'SYNTHESIZES' },

                    { source: 'Sum_of_Squares_Methods', target: 'Lyapunov_Function', type: 'FINDS' },
                    { source: 'Sum_of_Squares_Methods', target: 'Polynomial_Systems', type: 'APPLIES_TO' } // Added Polynomial_Systems node
                ]
            };

            // Add any missing nodes from links
            const existingNodeIds = new Set(graphData.nodes.map(n => n.id));
            const newNodesToAdd = new Set();
            graphData.links.forEach(link => {
                if (!existingNodeIds.has(link.source)) newNodesToAdd.add(link.source);
                if (!existingNodeIds.has(link.target)) newNodesToAdd.add(link.target);
            });
            newNodesToAdd.forEach(id => {
                if (!existingNodeIds.has(id)) {
                    graphData.nodes.push({ id: id, group: 'Implicit_Concept', description: 'Automatically added concept.' });
                }
            });


            // Get container dimensions with fallbacks
            let width = document.getElementById('graph').clientWidth;
            let height = document.getElementById('graph').clientHeight;
            // Provide a minimum fallback if dimensions are 0 initially
            if (width === 0) width = 800; // Default width
            if (height === 0) height = 600; // Default height

            const svg = d3.select("#graph").append("svg")
                .attr("width", "100%") // Ensure SVG takes full width of its container
                .attr("height", "100%") // Ensure SVG takes full height of its container
                .attr("viewBox", `0 0 ${width} ${height}`) // Use initially determined or fallback dimensions for viewBox
                .attr("preserveAspectRatio", "xMidYMid meet");

            // Define color scale for node groups
            const color = d3.scaleOrdinal(d3.schemeCategory10);

            // Create a force simulation
            const simulation = d3.forceSimulation(graphData.nodes)
                .force("link", d3.forceLink(graphData.links).id(d => d.id).distance(120)) // Link distance
                .force("charge", d3.forceManyBody().strength(-300)) // Node repulsion
                .force("center", d3.forceCenter(width / 2, height / 2)) // Center the graph
                .force("collision", d3.forceCollide().radius(d => 30)); // Prevent node overlap

            // Add zooming and panning
            const zoom = d3.zoom()
                .scaleExtent([0.1, 4]) // Zoom limits
                .on("zoom", (event) => {
                    g.attr("transform", event.transform);
                });

            const g = svg.append("g"); // Group for all elements, to apply zoom/pan
            svg.call(zoom);

            // Add links
            const link = g.append("g")
                .attr("class", "links")
                .selectAll("line")
                .data(graphData.links)
                .enter().append("line")
                .attr("class", "link");

            // Add link labels (relationship types)
            const linkText = g.append("g")
                .attr("class", "link-labels")
                .selectAll("text")
                .data(graphData.links)
                .enter().append("text")
                .attr("font-size", "8px")
                .attr("fill", "#64748b") /* Tailwind: text-slate-500 */
                .attr("dy", "0.35em")
                .attr("text-anchor", "middle")
                .text(d => d.type.replace(/_/g, ' ')); // Replace underscores for readability

            // Add nodes
            const node = g.append("g")
                .attr("class", "nodes")
                .selectAll("g")
                .data(graphData.nodes)
                .enter().append("g")
                .attr("class", "node")
                .call(d3.drag()
                    .on("start", dragstarted)
                    .on("drag", dragged)
                    .on("end", dragended));

            node.append("circle")
                .attr("r", 15) // Circle radius
                .attr("fill", d => color(d.group))
                .attr("class", "rounded-full shadow-md")
                .on("mouseover", function(event, d) {
                    d3.select("#selected-node-info").html(`
                        <p class="font-bold">${d.id.replace(/_/g, ' ')}</p>
                        <p>${d.description || 'No description available.'}</p>
                    `);
                    // Show tooltip
                    tooltip.html(`
                        <p class="font-bold">${d.id.replace(/_/g, ' ')}</p>
                        <p>${d.description || 'No description available.'}</p>
                    `).style("left", (event.pageX + 10) + "px")
                    .style("top", (event.pageY + 10) + "px")
                    .style("opacity", 1);
                })
                .on("mouseout", function(event, d) {
                    d3.select("#selected-node-info").text("Hover over a node to see its details.");
                    // Hide tooltip
                    tooltip.style("opacity", 0);
                });

            node.append("text")
                .attr("dy", 4) // Adjust text position vertically
                .text(d => d.id.replace(/_/g, ' '))
                .attr("font-weight", "bold");

            // Create a tooltip div
            const tooltip = d3.select("body").append("div")
                .attr("class", "tooltip");

            // Update positions on each simulation tick
            simulation.on("tick", () => {
                link
                    .attr("x1", d => d.source.x)
                    .attr("y1", d => d.source.y)
                    .attr("x2", d => d.target.x)
                    .attr("y2", d => d.target.y);

                linkText
                    .attr("x", d => (d.source.x + d.target.x) / 2)
                    .attr("y", d => (d.source.y + d.target.y) / 2);

                node
                    .attr("transform", d => `translate(${d.x},${d.y})`);
            });

            // Drag functions
            function dragstarted(event, d) {
                if (!event.active) simulation.alphaTarget(0.3).restart();
                d.fx = d.x;
                d.fy = d.y;
            }

            function dragged(event, d) {
                d.fx = event.x;
                d.fy = event.y;
            }

            function dragended(event, d) {
                if (!event.active) simulation.alphaTarget(0);
                // Optionally fix node position after drag
                // d.fx = null;
                // d.fy = null;
            }

            // Handle window resizing
            window.addEventListener('resize', () => {
                const newWidth = document.getElementById('graph').clientWidth;
                const newHeight = document.getElementById('graph').clientHeight;
                // Only update viewBox if new dimensions are valid
                if (newWidth > 0 && newHeight > 0) {
                    svg.attr("viewBox", `0 0 ${newWidth} ${newHeight}`);
                    simulation.force("center", d3.forceCenter(newWidth / 2, newHeight / 2));
                    simulation.alpha(0.3).restart();
                }
            });
        });
    </script>
</body>
</html>
