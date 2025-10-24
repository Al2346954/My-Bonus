import React, { useState } from 'react';
import { ChevronLeft, TrendingUp, Award, AlertCircle, Check, X, Minus } from 'lucide-react';

const ImprovedBonusTool = () => {
  const [currentView, setCurrentView] = useState('home'); // 'home' or 'comparison'
  const [activeTab, setActiveTab] = useState('B'); // Which bonus rule tab is active
  const [openDrawer, setOpenDrawer] = useState(null); // Which component drawer is open
  const [showWhyExplanation, setShowWhyExplanation] = useState(false); // Show explanation in hero card

  // Current month bonus data - Percentage-based
  const currentBonus = {
    status: 'ready',
    period: 'September 2025',
    paymentDate: 'October 31, 2025',
    isPending: true, // Will be paid at end of current month
    selectedRule: 'B',
    monthlySalary: 3140, // User's monthly salary for reference
    bonusA: {
      name: 'Bonus Rule A',
      totalPercent: 12.5, // Total percentage of monthly salary
      components: [
        {
          id: 'sales-a',
          name: 'Sales Performance',
          icon: '💰',
          weight: 50,
          achievementPercent: 68,
          earnedPercent: 6.8,
          maxPercent: 10,
          status: 'partial',
          minimumThreshold: { value: 5000, unit: '€ in sales', description: 'Minimum €5,000 in sales to qualify' },
          currentValue: 9800,
          nextMilestone: { target: 10000, units: 'sales', needed: 200, value: 0.2 },
          segments: [
            { name: '€5,000-€7,500', threshold: 5000, rate: '3% of salary', achieved: 7500, total: 7500, percentValue: 3, status: 'complete' },
            { name: '€7,501-€10,000', threshold: 7501, rate: '2% of salary', achieved: 2300, total: 2499, percentValue: 1.8, status: 'partial' },
            { name: '€10,001+', threshold: 10001, rate: '3% of salary', achieved: 0, total: 0, percentValue: 0, status: 'locked' }
          ]
        },
        {
          id: 'customer-a',
          name: 'Customer Satisfaction',
          icon: '⭐',
          weight: 30,
          achievementPercent: 83,
          earnedPercent: 2.5,
          maxPercent: 3,
          status: 'good',
          minimumThreshold: { value: 70, unit: '% satisfaction', description: 'Minimum 70% customer satisfaction' },
          currentValue: 83,
          nextMilestone: { target: 95, metric: 'satisfaction', needed: 12, value: 0.5 },
          segments: [
            { name: '70-84% satisfaction', threshold: 70, rate: '1.5% of salary', achieved: 83, total: 84, percentValue: 1.5, status: 'complete' },
            { name: '85-94% satisfaction', threshold: 85, rate: '1% of salary', achieved: 0, total: 94, percentValue: 1, status: 'partial' },
            { name: '95%+ satisfaction', threshold: 95, rate: '0.5% of salary', achieved: 0, total: 100, percentValue: 0, status: 'locked' }
          ]
        },
        {
          id: 'attendance-a',
          name: 'Attendance',
          icon: '📅',
          weight: 20,
          achievementPercent: 100,
          earnedPercent: 3.2,
          maxPercent: 4,
          status: 'excellent',
          minimumThreshold: { value: 18, unit: 'days/month', description: 'Minimum 18 days attendance' },
          currentValue: 20,
          segments: [
            { name: '18-19 days', threshold: 18, rate: '1% of salary', achieved: 19, total: 19, percentValue: 1, status: 'complete' },
            { name: '20-21 days', threshold: 20, rate: '2% of salary', achieved: 20, total: 21, percentValue: 2, status: 'complete' },
            { name: '22 days (perfect)', threshold: 22, rate: '1% of salary', achieved: 0, total: 22, percentValue: 0, status: 'locked' }
          ]
        }
      ]
    },
    bonusB: {
      name: 'Bonus Rule B',
      totalPercent: 15.2,
      components: [
        {
          id: 'sales-b',
          name: 'Sales Performance',
          icon: '💰',
          weight: 40,
          achievementPercent: 82,
          earnedPercent: 6.6,
          maxPercent: 8,
          status: 'good',
          minimumThreshold: { value: 5000, unit: '€ in sales', description: 'Minimum €5,000 in sales to qualify' },
          currentValue: 14200,
          nextMilestone: { target: 15000, units: 'sales', needed: 800, value: 0.4 },
          segments: [
            { name: '€5,000-€10,000', threshold: 5000, rate: '4% of salary', achieved: 10000, total: 10000, percentValue: 4, status: 'complete' },
            { name: '€10,001-€15,000', threshold: 10001, rate: '3% of salary', achieved: 4200, total: 5000, percentValue: 2.6, status: 'partial' },
            { name: '€15,001+', threshold: 15001, rate: '1% of salary', achieved: 0, total: 0, percentValue: 0, status: 'locked' }
          ]
        },
        {
          id: 'customer-b',
          name: 'Customer Satisfaction',
          icon: '⭐',
          weight: 30,
          achievementPercent: 90,
          earnedPercent: 5.4,
          maxPercent: 6,
          status: 'excellent',
          minimumThreshold: { value: 50, unit: 'surveys', description: 'Minimum 50 customer surveys' },
          currentValue: 58,
          segments: [
            { name: '50+ surveys collected', threshold: 50, rate: '3% of salary', achieved: 58, total: 50, percentValue: 3, status: 'complete' },
            { name: '4.2+ avg rating', threshold: 4.2, rate: '2% of salary', achieved: 4.6, total: 5, percentValue: 2, status: 'complete' },
            { name: '95%+ resolution rate', threshold: 95, rate: '1% of salary', achieved: 98, total: 100, percentValue: 0.4, status: 'partial' }
          ]
        },
        {
          id: 'attendance-b',
          name: 'Attendance',
          icon: '📅',
          weight: 15,
          achievementPercent: 91,
          earnedPercent: 1.4,
          maxPercent: 3,
          status: 'good',
          minimumThreshold: { value: 18, unit: 'days', description: 'Minimum 18 days attendance' },
          currentValue: 20,
          segments: [
            { name: '18-21 days', threshold: 18, rate: '1% of salary', achieved: 20, total: 21, percentValue: 1, status: 'complete' },
            { name: '100% punctuality', threshold: 100, rate: '2% of salary', achieved: 0, total: 100, percentValue: 0.4, status: 'partial' }
          ]
        },
        {
          id: 'operational-b',
          name: 'Operational Excellence',
          icon: '🎯',
          weight: 15,
          achievementPercent: 60,
          earnedPercent: 1.8,
          maxPercent: 3,
          status: 'partial',
          minimumThreshold: { value: 1, unit: 'audit criteria', description: 'Must meet basic safety standards' },
          currentValue: 2,
          segments: [
            { name: 'Safety compliance', threshold: 1, rate: '0.5% of salary', achieved: 'Yes', total: 1, percentValue: 0.5, status: 'complete' },
            { name: 'Team rating 4+', threshold: 4, rate: '0.25% of salary', achieved: 4.2, total: 5, percentValue: 0.25, status: 'complete' },
            { name: 'Store audit pass', threshold: 1, rate: '1.5% of salary', achieved: 0, total: 1, percentValue: 1, status: 'partial' }
          ]
        }
      ]
    }
  };

  const historicalBonuses = [
    { id: 'aug-2025', period: 'August 2025', paidDate: 'September 30, 2025', selectedRule: 'A', finalPercent: 14.2 },
    { id: 'jul-2025', period: 'July 2025', paidDate: 'August 31, 2025', selectedRule: 'B', finalPercent: 11.5 },
    { id: 'jun-2025', period: 'June 2025', paidDate: 'July 31, 2025', selectedRule: 'B', finalPercent: 16.2 },
    { id: 'may-2025', period: 'May 2025', paidDate: 'June 30, 2025', selectedRule: 'A', finalPercent: 13.8 },
    { id: 'apr-2025', period: 'April 2025', paidDate: 'May 31, 2025', selectedRule: 'B', finalPercent: 12.4 },
    { id: 'mar-2025', period: 'March 2025', paidDate: 'April 30, 2025', selectedRule: 'A', finalPercent: 15.6 },
    { id: 'feb-2025', period: 'February 2025', paidDate: 'March 31, 2025', selectedRule: 'B', finalPercent: 10.2 },
    { id: 'jan-2025', period: 'January 2025', paidDate: 'February 28, 2025', selectedRule: 'A', finalPercent: 14.8 },
    { id: 'dec-2024', period: 'December 2024', paidDate: 'January 31, 2025', selectedRule: 'B', finalPercent: 17.5 },
    { id: 'nov-2024', period: 'November 2024', paidDate: 'December 31, 2024', selectedRule: 'A', finalPercent: 13.1 },
    { id: 'oct-2024', period: 'October 2024', paidDate: 'November 30, 2024', selectedRule: 'B', finalPercent: 11.9 },
    { id: 'sep-2024', period: 'September 2024', paidDate: 'October 31, 2024', selectedRule: 'A', finalPercent: 12.7 }
  ];

  // Get the selected bonus data
  const selectedBonusData = currentBonus[`bonus${currentBonus.selectedRule}`];
  const alternativeBonusData = currentBonus[`bonus${currentBonus.selectedRule === 'A' ? 'B' : 'A'}`];

  // Status badge component
  const StatusBadge = ({ status }) => {
    const config = {
      complete: { icon: Check, color: 'bg-green-100 text-green-700', label: 'Complete' },
      partial: { icon: Minus, color: 'bg-yellow-100 text-yellow-700', label: 'Partial' },
      locked: { icon: X, color: 'bg-gray-100 text-gray-500', label: 'Not Achieved' },
      good: { icon: Check, color: 'bg-green-100 text-green-700', label: 'Good' },
      excellent: { icon: Check, color: 'bg-emerald-100 text-emerald-700', label: 'Excellent' },
      'needs-improvement': { icon: AlertCircle, color: 'bg-orange-100 text-orange-700', label: 'Needs Improvement' }
    };
    
    const { icon: Icon, color, label } = config[status] || config.partial;
    
    return (
      <span className={`inline-flex items-center gap-1 px-2 py-1 rounded-full text-xs font-medium ${color}`}>
        <Icon size={12} />
        {label}
      </span>
    );
  };

  // Component drawer - Side Panel Design
  const ComponentDrawer = ({ component, bonusRule }) => {
    if (openDrawer !== component.id) return null;

    return (
      <>
        {/* Backdrop */}
        <div 
          className="fixed inset-0 bg-black bg-opacity-30 z-50 transition-opacity"
          onClick={() => setOpenDrawer(null)}
        />
        
        {/* Side Panel */}
        <div className="fixed right-0 top-0 bottom-0 w-full sm:w-96 bg-gradient-to-br from-green-50 to-emerald-50 shadow-2xl z-50 overflow-y-auto">
          {/* Header */}
          <div className="sticky top-0 bg-white border-b border-gray-200 p-4">
            <button
              onClick={() => setOpenDrawer(null)}
              className="absolute top-4 right-4 text-gray-400 hover:text-gray-600 transition-colors"
            >
              <X size={24} />
            </button>
            
            <div className="pr-8">
              <div className="flex items-center gap-3 mb-2">
                <span className="text-3xl">{component.icon}</span>
                <h3 className="font-bold text-gray-900 text-lg">{component.name}</h3>
              </div>
              
              {/* Bonus contribution badge */}
              <div className="inline-flex items-center gap-2 text-sm">
                <span className="text-gray-600">Bonus contribution:</span>
                <span className="px-3 py-1 bg-green-500 text-white rounded-full font-semibold">
                  {component.earnedPercent}%/{component.maxPercent}%
                </span>
              </div>
            </div>
          </div>

          {/* Content */}
          <div className="p-6">
            {/* Achievement Summary */}
            <div className="mb-6 p-4 bg-white rounded-lg border border-green-200 shadow-sm">
              <div className="flex items-center justify-between mb-2">
                <span className="text-sm font-medium text-gray-700">Your Achievement</span>
                <StatusBadge status={component.status} />
              </div>
              <div className="text-3xl font-bold text-gray-900 mb-1">
                {component.earnedPercent}% of salary
              </div>
              <div className="text-sm text-gray-600">
                {component.achievementPercent}% of maximum ({component.maxPercent}% of salary)
              </div>
              <div className="text-xs text-gray-500 mt-2">
                ≈ €{Math.round((component.earnedPercent / 100) * currentBonus.monthlySalary)}
              </div>
            </div>

            {/* Vertical Progress Visualization */}
            <div className="mb-6">
              <h4 className="font-semibold text-gray-900 mb-4">Progress Tiers</h4>
              
              <div className="relative">
                {/* Vertical gradient bar background */}
                <div 
                  className="absolute left-1/2 transform -translate-x-1/2 w-2 rounded-full overflow-hidden" 
                  style={{ 
                    height: `${(component.segments.length * 120) - 20}px`,
                    background: 'linear-gradient(to bottom, #10b981 0%, #fbbf24 50%, #ef4444 100%)'
                  }}
                >
                  {/* Active portion indicator */}
                  <div 
                    className="absolute bottom-0 w-full bg-gradient-to-t from-red-500 to-transparent opacity-60"
                    style={{ height: `${(component.achievementPercent / 100) * 100}%` }}
                  />
                </div>

                {/* Tiers */}
                <div className="relative space-y-6 py-4">
                  {[...component.segments].reverse().map((segment, idx) => {
                    return (
                      <div key={idx} className="relative flex items-center">
                        {/* Left side - Labels */}
                        <div className="flex-1 text-right pr-6">
                          <div className={`text-sm font-semibold ${
                            segment.status === 'complete' ? 'text-gray-900' :
                            segment.status === 'partial' ? 'text-gray-700' :
                            'text-gray-400'
                          }`}>
                            {segment.name}
                          </div>
                          <div className={`text-xs mt-1 ${
                            segment.status === 'complete' ? 'text-gray-600' :
                            segment.status === 'partial' ? 'text-gray-500' :
                            'text-gray-400'
                          }`}>
                            {segment.rate}
                          </div>
                        </div>

                        {/* Center - Checkpoint */}
                        <div className="relative z-10">
                          <div className={`w-10 h-10 rounded-full border-4 flex items-center justify-center shadow-lg ${
                            segment.status === 'complete' ? 'bg-green-500 border-green-600' :
                            segment.status === 'partial' ? 'bg-yellow-400 border-yellow-500' :
                            'bg-gray-200 border-gray-300'
                          }`}>
                            {segment.status === 'complete' && <Check size={20} className="text-white" />}
                            {segment.status === 'partial' && <Minus size={20} className="text-white" />}
                            {segment.status === 'locked' && <X size={16} className="text-gray-400" />}
                          </div>
                        </div>

                        {/* Right side - Value earned */}
                        <div className="flex-1 pl-6">
                          <div className={`text-2xl font-bold ${
                            segment.status === 'complete' ? 'text-green-600' :
                            segment.status === 'partial' ? 'text-yellow-600' :
                            'text-gray-300'
                          }`}>
                            {segment.percentValue}%
                          </div>
                          <div className="text-xs text-gray-400 mt-0.5">
                            ≈ €{Math.round((segment.percentValue / 100) * currentBonus.monthlySalary)}
                          </div>
                        </div>
                      </div>
                    );
                  })}
                </div>
              </div>
            </div>

            {/* Achievement Details */}
            <div className="bg-white rounded-lg border border-gray-200 p-4">
              <h4 className="font-semibold text-gray-900 mb-3">Achievement Status</h4>
              <div className="space-y-2 text-sm text-gray-700">
                {component.segments.map((segment, idx) => (
                  <div key={idx} className="flex items-start gap-2">
                    <div className={`w-5 h-5 rounded-full flex items-center justify-center flex-shrink-0 mt-0.5 ${
                      segment.status === 'complete' ? 'bg-green-100' :
                      segment.status === 'partial' ? 'bg-yellow-100' :
                      'bg-gray-100'
                    }`}>
                      {segment.status === 'complete' && <Check size={12} className="text-green-600" />}
                      {segment.status === 'partial' && <Minus size={12} className="text-yellow-600" />}
                      {segment.status === 'locked' && <X size={10} className="text-gray-400" />}
                    </div>
                    <div>
                      <span className="font-medium">{segment.name}:</span>{' '}
                      {segment.status === 'complete' ? 'Achieved' :
                       segment.status === 'partial' ? `Partial (${segment.achieved}${typeof segment.achieved === 'number' ? ` / ${segment.total}` : ''})` :
                       'Not achieved'}
                    </div>
                  </div>
                ))}
              </div>
            </div>
          </div>
        </div>
      </>
    );
  };

  // Weight distribution visualization - Horizontal stacked bar
  const WeightDistribution = ({ components }) => {
    const colorClasses = [
      { bg: 'bg-blue-500', text: 'text-white' },
      { bg: 'bg-red-400', text: 'text-white' },
      { bg: 'bg-yellow-400', text: 'text-gray-900' },
      { bg: 'bg-emerald-500', text: 'text-white' },
      { bg: 'bg-indigo-500', text: 'text-white' }
    ];
    
    return (
      <div className="w-full">
        <div className="flex h-12 rounded-lg overflow-hidden shadow-md">
          {components.map((component, idx) => (
            <div
              key={component.id}
              className={`${colorClasses[idx % colorClasses.length].bg} ${colorClasses[idx % colorClasses.length].text} flex items-center justify-center font-semibold text-xs cursor-pointer hover:opacity-90 transition-opacity`}
              style={{ width: `${component.weight}%` }}
              onClick={() => setOpenDrawer(component.id)}
              title={`${component.name}: ${component.weight}%`}
            >
              <span className="px-2 text-center">
                <div className="text-sm">{component.icon}</div>
                <div className="text-xs">{component.weight}%</div>
              </span>
            </div>
          ))}
        </div>
        <div className="mt-2 flex flex-wrap gap-2 text-xs">
          {components.map((component, idx) => (
            <div key={component.id} className="flex items-center gap-1">
              <div className={`w-3 h-3 rounded ${colorClasses[idx % colorClasses.length].bg}`}></div>
              <span className="text-gray-700">{component.name}</span>
            </div>
          ))}
        </div>
      </div>
    );
  };

  // Comparison Detail Page
  if (currentView === 'comparison') {
    const activeBonusData = currentBonus[`bonus${activeTab}`];
    
    return (
      <div className="min-h-screen bg-gray-50">
        {/* Header */}
        <div className="bg-white border-b border-gray-200 sticky top-0 z-40">
          <div className="max-w-5xl mx-auto px-4 py-3">
            {/* Back button - more visible */}
            <button
              onClick={() => setCurrentView('home')}
              className="inline-flex items-center gap-2 text-gray-700 hover:text-gray-900 bg-gray-100 hover:bg-gray-200 px-3 py-2 rounded-lg transition-colors mb-3 font-medium"
            >
              <ChevronLeft size={18} />
              <span>Back to My Bonus</span>
            </button>
            
            {/* Title and period - compact */}
            <div className="mb-3">
              <h1 className="text-xl font-bold text-gray-900">Bonus Calculation Details</h1>
              <p className="text-sm text-gray-600">{currentBonus.period}</p>
            </div>
            
            {/* Tabs - more compact */}
            <div className="flex gap-2">
              <button
                onClick={() => setActiveTab('A')}
                className={`flex-1 px-4 py-2 rounded-lg font-medium transition-colors ${
                  activeTab === 'A'
                    ? 'bg-gray-100 text-gray-900 border-b-2 border-blue-600'
                    : 'bg-white text-gray-600 hover:text-gray-900 hover:bg-gray-50'
                }`}
              >
                <div className="flex items-center justify-center gap-2">
                  <span>Bonus A</span>
                  {currentBonus.selectedRule === 'A' && (
                    <Award size={14} className="text-green-600" />
                  )}
                </div>
                <div className="text-xs mt-0.5">{currentBonus.bonusA.totalPercent}% of salary</div>
              </button>
              
              <button
                onClick={() => setActiveTab('B')}
                className={`flex-1 px-4 py-2 rounded-lg font-medium transition-colors ${
                  activeTab === 'B'
                    ? 'bg-gray-100 text-gray-900 border-b-2 border-blue-600'
                    : 'bg-white text-gray-600 hover:text-gray-900 hover:bg-gray-50'
                }`}
              >
                <div className="flex items-center justify-center gap-2">
                  <span>Bonus B</span>
                  {currentBonus.selectedRule === 'B' && (
                    <Award size={14} className="text-green-600" />
                  )}
                </div>
                <div className="text-xs mt-0.5">{currentBonus.bonusB.totalPercent}% of salary</div>
              </button>
            </div>
          </div>
        </div>

        {/* Tab Content */}
        <div className="max-w-5xl mx-auto p-4">
          {/* Total */}
          <div className="mb-4 p-5 bg-gradient-to-br from-blue-600 to-indigo-700 rounded-lg text-white shadow-lg">
            <p className="text-blue-100 text-xs mb-1">Total for this model</p>
            <p className="text-4xl font-bold">{activeBonusData.totalPercent}% of salary</p>
            <p className="text-blue-100 text-sm mt-1">≈ €{Math.round((activeBonusData.totalPercent / 100) * currentBonus.monthlySalary)}</p>
            {currentBonus.selectedRule === activeTab && (
              <div className="mt-2 inline-flex items-center gap-2 bg-white bg-opacity-20 px-3 py-1 rounded-full text-xs">
                <Award size={14} />
                <span>This model was used</span>
              </div>
            )}
          </div>

          {/* Weight Distribution */}
          <div className="mb-6 bg-white rounded-lg border border-gray-200 p-4">
            <h3 className="font-semibold text-gray-900 mb-2 text-sm">Component Weights</h3>
            <p className="text-xs text-gray-600 mb-3">
              Click any component to see detailed breakdown.
            </p>
            <WeightDistribution components={activeBonusData.components} />
          </div>

          {/* Components as cards */}
          <div className="space-y-4">
            <h3 className="font-semibold text-gray-900 text-sm">Performance Breakdown</h3>
            {activeBonusData.components.map((component) => (
              <div
                key={component.id}
                onClick={() => setOpenDrawer(component.id)}
                className="bg-white rounded-lg border border-gray-200 p-5 hover:border-blue-300 hover:shadow-md transition-all cursor-pointer"
              >
                {/* Header with icon, name, and earned percentage */}
                <div className="flex items-start justify-between mb-4">
                  <div className="flex items-start gap-3">
                    <span className="text-2xl">{component.icon}</span>
                    <div>
                      <h4 className="font-semibold text-gray-900">{component.name}</h4>
                      <p className="text-sm text-gray-600">Weight: {component.weight}%</p>
                      {component.minimumThreshold && (
                        <p className="text-xs text-gray-500 mt-1">
                          Current: <span className="font-medium">{component.currentValue} {component.minimumThreshold.unit}</span>
                          {component.currentValue >= component.minimumThreshold.value && ' ✓'}
                        </p>
                      )}
                    </div>
                  </div>
                  <div className="text-right">
                    <div className="text-3xl font-bold text-gray-900">{component.earnedPercent}%</div>
                    <div className="text-xs text-gray-500 mb-2">of salary</div>
                    <StatusBadge status={component.status} />
                  </div>
                </div>

                {/* Visual tier progress bar */}
                <div className="mb-3">
                  <div className="flex h-8 rounded-lg overflow-hidden border border-gray-200">
                    {component.segments.map((segment, idx) => {
                      const segmentWidth = (segment.percentValue / component.maxPercent) * 100;
                      const bgColor = 
                        segment.status === 'complete' ? 'bg-green-500' :
                        segment.status === 'partial' ? 'bg-yellow-400' :
                        'bg-gray-200';
                      
                      return (
                        <div
                          key={idx}
                          className={`${bgColor} flex items-center justify-center text-xs font-medium ${
                            segment.status === 'locked' ? 'text-gray-400' : 'text-white'
                          }`}
                          style={{ width: `${segmentWidth}%` }}
                          title={`${segment.name}: ${segment.percentValue}%`}
                        >
                          {segmentWidth > 15 && (
                            <span className="px-1 truncate">{segment.percentValue}%</span>
                          )}
                        </div>
                      );
                    })}
                  </div>
                  
                  {/* Tier labels */}
                  <div className="flex justify-between mt-2 text-xs text-gray-600">
                    {component.segments.map((segment, idx) => (
                      <div key={idx} className="flex-1 text-center">
                        {idx === 0 && <div className="font-medium">{segment.name.split(':')[0]}</div>}
                      </div>
                    ))}
                  </div>
                </div>

                {/* Achievement summary */}
                <div className="text-sm text-gray-600">
                  <span className="font-semibold text-gray-900">{component.achievementPercent}%</span> achieved • 
                  <span className="font-semibold text-gray-900"> {component.earnedPercent}%</span> of 
                  <span className="font-semibold text-gray-900"> {component.maxPercent}%</span> max
                </div>
              </div>
            ))}
          </div>
        </div>

        {/* Drawers for each component */}
        {activeBonusData.components.map((component) => (
          <ComponentDrawer key={component.id} component={component} bonusRule={activeTab} />
        ))}
      </div>
    );
  }

  // Home Page
  return (
    <div className="min-h-screen bg-gray-50 p-6">
      <div className="max-w-5xl mx-auto">
        <h1 className="text-2xl font-bold text-gray-900 mb-6">My Bonus</h1>

        {currentBonus.status === 'generating' ? (
          <div className="bg-blue-50 border-l-4 border-blue-500 rounded-lg p-6">
            <div className="flex items-start gap-3">
              <AlertCircle size={24} className="text-blue-600 flex-shrink-0 mt-1" />
              <div>
                <h3 className="font-semibold text-blue-900 mb-2">We're generating your bonus</h3>
                <p className="text-blue-800">
                  Your bonus is usually ready by the 20th of every month. Please check back soon.
                </p>
              </div>
            </div>
          </div>
        ) : (
          <>
            {/* PROTAGONIST: Final Bonus Card */}
            <div className="bg-gradient-to-br from-green-600 to-emerald-700 rounded-lg shadow-xl p-8 mb-4 text-white">
              <div className="text-center">
                {/* Period info - more compact */}
                <p className="text-green-100 text-sm mb-4">September 2025 performance</p>
                
                {/* Main bonus display */}
                <div className="mb-4">
                  <div className="flex items-baseline justify-center gap-3">
                    <div className="text-7xl font-bold">{selectedBonusData.totalPercent}%</div>
                    <div className="text-4xl font-semibold text-green-200">
                      €{Math.round((selectedBonusData.totalPercent / 100) * currentBonus.monthlySalary)}
                    </div>
                  </div>
                </div>
                
                {/* Payment date - subtle */}
                <p className="text-green-100 text-sm">
                  {currentBonus.isPending ? 'Payment on ' : 'Paid on '}{currentBonus.paymentDate}
                </p>
                
                {/* Action Buttons */}
                <div className="mt-6 flex flex-col sm:flex-row items-center justify-center gap-2">
                  {/* PRIMARY: Calculation Comparison Button */}
                  <button
                    onClick={() => {
                      setCurrentView('comparison');
                      setActiveTab(currentBonus.selectedRule);
                    }}
                    className="bg-white text-green-700 px-5 py-2.5 rounded-lg font-semibold hover:bg-green-50 transition-colors shadow-lg inline-flex items-center justify-center gap-2 text-sm"
                  >
                    <TrendingUp size={18} />
                    How was this calculated?
                  </button>
                  
                  {/* SECONDARY: Why this amount button */}
                  <button
                    onClick={() => setShowWhyExplanation(!showWhyExplanation)}
                    className="bg-white bg-opacity-20 text-white px-5 py-2.5 rounded-lg font-medium hover:bg-opacity-30 transition-colors inline-flex items-center justify-center gap-2 text-sm"
                  >
                    <AlertCircle size={16} />
                    Why {selectedBonusData.totalPercent}%?
                  </button>
                </div>
                
                {/* Expandable explanation */}
                {showWhyExplanation && (
                  <div className="mt-5 pt-5 border-t border-green-500 border-opacity-30">
                    <p className="text-green-50 text-sm mb-4 text-left">
                      We calculated your bonus using two models and selected <strong className="text-white">Bonus {currentBonus.selectedRule}</strong> because it gave you <strong className="text-white">{Math.abs(selectedBonusData.totalPercent - alternativeBonusData.totalPercent).toFixed(1)}%</strong> more.
                    </p>
                    <div className="flex gap-3">
                      <div className="flex-1 p-3 bg-white bg-opacity-10 rounded-lg backdrop-blur-sm text-center">
                        <div className="text-green-100 text-xs mb-1">Rule A</div>
                        <div className="font-bold text-white text-2xl">{currentBonus.bonusA.totalPercent}%</div>
                      </div>
                      <div className="flex-1 p-3 bg-white bg-opacity-10 rounded-lg backdrop-blur-sm text-center">
                        <div className="text-green-100 text-xs mb-1">Rule B</div>
                        <div className="font-bold text-white text-2xl">{currentBonus.bonusB.totalPercent}%</div>
                      </div>
                    </div>
                  </div>
                )}
              </div>
            </div>

            {/* Historical bonuses */}
            <div>
              <h2 className="text-xl font-bold text-gray-900 mb-2">Previous Months</h2>
              <p className="text-sm text-gray-600 mb-4">
                Your bonuses from the last 12 months. Each bonus is based on the previous month's performance and paid at month-end.
              </p>
              <div className="space-y-3">
                {historicalBonuses.map((bonus) => (
                  <div key={bonus.id} className="bg-white rounded-lg border border-gray-200 p-5 hover:border-gray-300 transition-colors">
                    <div className="flex items-center justify-between">
                      <div>
                        <div className="font-semibold text-gray-900">{bonus.period} performance</div>
                        <div className="text-sm text-gray-600 mt-1">
                          Paid on {bonus.paidDate} • Rule {bonus.selectedRule}
                        </div>
                      </div>
                      <div className="text-right">
                        <div className="text-2xl font-bold text-gray-900">
                          {bonus.finalPercent}%
                        </div>
                        <div className="text-sm text-gray-500">
                          ≈ €{Math.round((bonus.finalPercent / 100) * currentBonus.monthlySalary)}
                        </div>
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          </>
        )}
      </div>
    </div>
  );
};

export default ImprovedBonusTool;
